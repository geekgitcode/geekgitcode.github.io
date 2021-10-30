---
layout: post
title: '信息论'
date: 2021-10-10
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/deeplearning.jpeg'
tags: DL
---

> Information Theory


#### 信息和熵

自信息：（描述随机事件的信息量（不确定性））

![image-20211012171819188](https://gitee.com/GoPrime/imagecloud/raw/6ebace0938e95d1a9806639013d445d2c721b655/img/image-20211012171819188.png)

熵：一个随机变量的概率空间中，所包含的所有随机事件自信息量的数学期望

![image-20211012172134129](https://gitee.com/GoPrime/imagecloud/raw/dcbcb4f18d2991da2641c7676f258cd279acd407/img/image-20211012172134129.png)

意味着**遵循这个分布的事件**，所产生的**期望信息总量**。通常这也意味着对此概率分布进行编码所需的最小比特（bit, 使用2为底的对数）或奈特数（nats, 使用e为底的对数）。

**特性：**

- 接近均匀分布的概率分布具有较高的熵
- 接近确定性的分布 (输出几 乎可以确定) 具有较低的熵

#### 条件熵

给定一个事件，一个随机变量的条件熵

**1.给定一个事件，一个随机变量的条件熵：**

假定给定事件 $x_i$，则随机变量 $Y$ 的条件熵为

![image-20211030175701930](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030175701930.png)

**2.已知一个随机变量，另一个随机变量的条件熵：**

假定已知随机变量 $X$，则随机变量 $Y$ 的条件熵可以写为 ![image-20211030175743289](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030175743289.png) ，即

![image-20211030175806269](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030175806269.png)利用条件概率合并后，即得到 ![image-20211030175839077](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030175839077.png)，条件熵 ![image-20211030175856157](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030175856157.png) 表示在已知 $Y$ 的条件下，随机变量 $X$ 的不确定度。



#### 联合熵

两个随机变量的联合熵定义为**两个随机变量联合概率的自信息的数学期望**

![image-20211030175945863](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030175945863.png)

![img](https://pic4.zhimg.com/80/v2-d2f141d2bd4656addeec9e34ff90023f_1440w.jpg)

#### 互信息

两个随机事件之间的互信息定义为，已知一个事件，对另一个事件不确定性的削减量。例如，我们现在已知一个事件 $x_i$ ，在这个前提下，另一个事件 $y_i$ 的不确定性（信息量）就受到了削减。同样，已知事件 $y_i$，事件 $x_i$ 的不确定性（信息量）也会受到削减。这两个削减量是相等的，我们把这个削减量称为两个事件之间的互信息，记作 ![image-20211030180102967](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030180102967.png)

![image-20211030180116969](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211030180116969.png)

#### 交叉熵

在信息论中，交叉熵是表示两个概率分布p,q，其中p表示真实分布，q表示非真实分布，在相同的一组事件中，其中，用非真实分布q来表示某个事件发生所需要的平均比特数。

假设现在有一个样本集中两个概率分布p,q，其中p为真实分布，q为非真实分布。假如，按照真实分布p来衡量识别一个样本所需要的编码长度的期望为：

![image-20211012173655060](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211012173655060.png)

但是，如果采用错误的分布q来表示来自真实分布p的平均编码长度，则应该是：

![image-20211012173723735](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211012173723735.png)

此时就将H(p,q)称之为交叉熵。交叉熵的计算方式如下：

对于离散变量采用以下的方式计算：

![image-20211012173755151](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211012173755151.png)

对于连续变量采用以下的方式计算：![image-20211012173812853](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211012173812853.png)

#### KL散度

将由q得到的平均编码长度比由p得到的平均编码长度多出的bit数称为“相对熵”，也叫做KL散度（Kullback–Leibler divergence）：(q是近似分布，p是拟合分布)

![image-20211012175051819](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20211012175051819.png)

交叉熵就是真值分布的熵与KL散度的和, 而真值的熵是确定的,与模型的参数 $θ$ 无关,所以梯度下降求导时,  $$\nabla D_{KL}(P||Q) = \nabla H(P,Q)$$， 即最小化交叉熵与最小化KL散度是一样的



#### 相关参考：

[知乎](https://zhuanlan.zhihu.com/p/143105854)

[知乎](https://www.zhihu.com/column/c_1348594680624488450)

[CSDN](https://blog.csdn.net/zhangyuexiang123/article/details/99712589?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link)