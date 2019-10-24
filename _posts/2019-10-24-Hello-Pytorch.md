---
layout: post
title: 'Hello Pytorch'
date: 2019-10-24
author: downeyking
cover: 'https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1571908035&di=a0390f87e4042cc6766097dc4c226dde&src=http://crawl.nosdn.127.net/img/7f713babca4d526f2d64ffa96cd2480a.png'    
tags: python,machine-learning
---

- # MODEL PARALLEL BEST PRACTICES

  Model parallel is widely-used in distributed training techniques. Previous posts have explained how to use [DataParallel](https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html) to train a neural network on multiple GPUs; this feature replicates the same model to all GPUs, where each GPU consumes a different partition of the input data. Although it can significantly accelerate the training process, it does not work for some use cases where the model is too large to fit into a single GPU. This post shows how to solve that problem by using **model parallel**, which, in contrast to `DataParallel`, splits a single model onto different GPUs, rather than replicating the entire model on each GPU (to be concrete, say a model `m` contains 10 layers: when using `DataParallel`, each GPU will have a replica of each of these 10 layers, whereas when using model parallel on two GPUs, each GPU could host 5 layers).

  The high-level idea of model parallel is to place different sub-networks of a model onto different devices, and implement the `forward` method accordingly to move intermediate outputs across devices. As only part of a model operates on any individual device, a set of devices can collectively serve a larger model. In this post, we will not try to construct huge models and squeeze them into a limited number of GPUs. Instead, this post focuses on showing the idea of model parallel. It is up to the readers to apply the ideas to real-world applications.

  ## Basic Usage

  Let us start with a toy model that contains two linear layers. To run this model on two GPUs, simply put each linear layer on a different GPU, and move inputs and intermediate outputs to match the layer devices accordingly.

  ```python
  		p{ color: red }
  import torch
  import torch.nn as nn
  import torch.optim as optim
  
  
  class ToyModel(nn.Module):
      def __init__(self):
          super(ToyModel, self).__init__()
          self.net1 = torch.nn.Linear(10, 10).to('cuda:0')
          self.relu = torch.nn.ReLU()
          self.net2 = torch.nn.Linear(10, 5).to('cuda:1')
  
      def forward(self, x):
          x = self.relu(self.net1(x.to('cuda:0')))
          return self.net2(x.to('cuda:1'))
  ```

  Note that, the above `ToyModel` looks very similar to how one would implement it on a single GPU, except the five `to(device)` calls which place linear layers and tensors on proper devices. That is the only place in the model that requires changes. The `backward()` and `torch.optim` will automatically take care of gradients as if the model is on one GPU. You only need to make sure that the labels are on the same device as the outputs when calling the loss function.

  ```python
  		p { color: red }
  model = ToyModel()
  loss_fn = nn.MSELoss()
  optimizer = optim.SGD(model.parameters(), lr=0.001)
  
  optimizer.zero_grad()
  outputs = model(torch.randn(20, 10))
  labels = torch.randn(20, 5).to('cuda:1')
  loss_fn(outputs, labels).backward()
  optimizer.step()
  ```

  ## Apply Model Parallel to Existing Modules

  It is also possible to run an existing single-GPU module on multiple GPUs with just a few lines of changes. The code below shows how to decompose `torchvision.models.reset50()` to two GPUs. The idea is to inherit from the existing `ResNet` module, and split the layers to two GPUs during construction. Then, override the `forward` method to stitch two sub-networks by moving the intermediate outputs accordingly.

  ```python
  		p { color: red }
  from torchvision.models.resnet import ResNet, Bottleneck
  
  num_classes = 1000
  
  
  class ModelParallelResNet50(ResNet):
      def __init__(self, *args, **kwargs):
          super(ModelParallelResNet50, self).__init__(
              Bottleneck, [3, 4, 6, 3], num_classes=num_classes, *args, **kwargs)
  
          self.seq1 = nn.Sequential(
              self.conv1,
              self.bn1,
              self.relu,
              self.maxpool,
  
              self.layer1,
              self.layer2
          ).to('cuda:0')
  
          self.seq2 = nn.Sequential(
              self.layer3,
              self.layer4,
              self.avgpool,
          ).to('cuda:1')
  
          self.fc.to('cuda:1')
  
      def forward(self, x):
          x = self.seq2(self.seq1(x).to('cuda:1'))
          return self.fc(x.view(x.size(0), -1))
  ```

  The above implementation solves the problem for cases where the model is too large to fit into a single GPU. However, you might have already noticed that it will be slower than running it on a single GPU if your model fits. It is because, at any point in time, only one of the two GPUs are working, while the other one is sitting there doing nothing. The performance further deteriorates as the intermediate outputs need to be copied from `cuda:0` to `cuda:1` between `layer2` and `layer3`.

  Let us run an experiment to get a more quantitative view of the execution time. In this experiment, we train `ModelParallelResNet50` and the existing `torchvision.models.reset50()` by running random inputs and labels through them. After the training, the models will not produce any useful predictions, but we can get a reasonable understanding of the execution times.

  ```python
  		p { color: red }
  import torchvision.models as models
  
  num_batches = 3
  batch_size = 120
  image_w = 128
  image_h = 128
  
  
  def train(model):
      model.train(True)
      loss_fn = nn.MSELoss()
      optimizer = optim.SGD(model.parameters(), lr=0.001)
  
      one_hot_indices = torch.LongTensor(batch_size) \
                             .random_(0, num_classes) \
                             .view(batch_size, 1)
  
      for _ in range(num_batches):
          # generate random inputs and labels
          inputs = torch.randn(batch_size, 3, image_w, image_h)
          labels = torch.zeros(batch_size, num_classes) \
                        .scatter_(1, one_hot_indices, 1)
  
          # run forward pass
          optimizer.zero_grad()
          outputs = model(inputs.to('cuda:0'))
  
          # run backward pass
          labels = labels.to(outputs.device)
          loss_fn(outputs, labels).backward()
          optimizer.step()
  ```

  The `train(model)` method above uses `nn.MSELoss` as the loss function, and `optim.SGD` as the optimizer. It mimics training on `128 X 128` images which are organized into 3 batches where each batch contains 120 images. Then, we use `timeit` to run the `train(model)` method 10 times and plot the execution times with standard deviations.

  ```python
  		p { color: red }
  import matplotlib.pyplot as plt
  plt.switch_backend('Agg')
  import numpy as np
  import timeit
  
  num_repeat = 10
  
  stmt = "train(model)"
  
  setup = "model = ModelParallelResNet50()"
  # globals arg is only available in Python 3. In Python 2, use the following
  # import __builtin__
  # __builtin__.__dict__.update(locals())
  mp_run_times = timeit.repeat(
      stmt, setup, number=1, repeat=num_repeat, globals=globals())
  mp_mean, mp_std = np.mean(mp_run_times), np.std(mp_run_times)
  
  setup = "import torchvision.models as models;" + \
          "model = models.resnet50(num_classes=num_classes).to('cuda:0')"
  rn_run_times = timeit.repeat(
      stmt, setup, number=1, repeat=num_repeat, globals=globals())
  rn_mean, rn_std = np.mean(rn_run_times), np.std(rn_run_times)
  
  
  def plot(means, stds, labels, fig_name):
      fig, ax = plt.subplots()
      ax.bar(np.arange(len(means)), means, yerr=stds,
             align='center', alpha=0.5, ecolor='red', capsize=10, width=0.6)
      ax.set_ylabel('ResNet50 Execution Time (Second)')
      ax.set_xticks(np.arange(len(means)))
      ax.set_xticklabels(labels)
      ax.yaxis.grid(True)
      plt.tight_layout()
      plt.savefig(fig_name)
      plt.close(fig)
  
  
  plot([mp_mean, rn_mean],
       [mp_std, rn_std],
       ['Model Parallel', 'Single GPU'],
       'mp_vs_rn.png')
  ```

![mp_vs_rn](C:\Users\74116\Desktop\mp_vs_rn.png)