---

layout: post
title: '激活函数'
date: 2021-03-26
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/deeplearning.jpeg'
tags: DL
---

> Activation function

**神经网络需要激活函数的原因：**首先数据的分布绝大多数是非线性的，而一般神经网络的计算是线性的，引入激活函数，是在神经网络中引入非线性，强化网络的学习能力。所以激活函数的最大特点就是非线性。

### **常见激活函数：**

#### **Sigmoid函数:**

​										$f(z)=\frac{1}{1+e^{-z}}$



![image-20210326214602360](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210326214602360.png)

特点：
它能够把输入的连续实值变换为0和1之间的输出，特别的，如果是非常大的负数，那么输出就是0；如果是非常大的正数，输出就是1.

优点：平滑、易于求导。

缺点：

1. 激活函数计算量大（在正向传播和反向传播中都包含幂运算和除法）；
2. 反向传播求误差梯度时，求导涉及除法；
3. Sigmoid导数取值范围是[0, 0.25]，由于神经网络反向传播时的“链式反应”，很容易就会出现梯度消失的情况。例如对于一个10层的网络， 根据![[公式]](https://www.zhihu.com/equation?tex=0.25%5E%7B10%7D%5Capprox0.000000954)，第10层的误差相对第一层卷积的参数![[公式]](https://www.zhihu.com/equation?tex=W_1)的梯度将是一个非常小的值，这就是所谓的“梯度消失”。
4. Sigmoid的输出不是0均值（即zero-centered）；这会导致后一层的神经元将得到上一层输出的非0均值的信号作为输入，随着网络的加深，会改变数据的原始分布。

#### **tanh函数:**

tanh为双曲正切函数，其英文读作Hyperbolic Tangent。tanh和 sigmoid 相似，都属于饱和激活函数，区别在于输出值范围由 (0,1) 变为了 (-1,1)，可以把 tanh 函数看做是 sigmoid 向下平移和拉伸后的结果。

​						$\tanh (x)=\frac{e^{x}-e^{-x}}{e^{x}+e^{-x}}$

![image-20210326214703247](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210326214703247.png)

相比Sigmoid函数，

1. tanh的输出范围时(-1, 1)，解决了Sigmoid函数的不是zero-centered输出问题；
2. 幂运算的问题仍然存在；
3. tanh导数范围在(0, 1)之间，相比sigmoid的(0, 0.25)，梯度消失（gradient vanishing）问题会得到缓解，但仍然还会存在。

#### **Relu(Rectified Linear Unit)函数——修正线性单元函数**

​					$\operatorname{ReLU}(z)=\left\{\begin{array}{l}z, z>0 \\ 0, \text { otherwise }\end{array}\right.$

![image-20210326215025538](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210326215025538.png)

从上图可知，ReLU的有效导数是常数1，解决了深层网络中出现的梯度消失问题，也就使得深层网络可训练。**同时ReLU又是非线性函数，所谓非线性，就是一阶导数不为常数；对ReLU求导，在输入值分别为正和为负的情况下，导数是不同的，即ReLU的导数不是常数，所以ReLU是非线性的（只是不同于Sigmoid和tanh，relu的非线性不是光滑的）。**

**ReLU在x>0下，导数为常数1的特点：**

导数为常数1的好处就是在“链式反应”中不会出现梯度消失，但梯度下降的强度就完全取决于权值的乘积，这样就可能会出现梯度爆炸问题。解决这类问题：一是控制权值，让它们在（0，1）范围内；二是做梯度裁剪，控制梯度下降强度，如ReLU(x)=min(6, max(0,x))

**ReLU在x<0下，输出置为0的特点：**

描述该特征前，需要明确深度学习的目标：深度学习是根据大批量样本数据，从错综复杂的数据关系中，找到关键信息（关键特征）。换句话说，就是把密集矩阵转化为稀疏矩阵，保留数据的关键信息，去除噪音，这样的模型就有了鲁棒性。ReLU将x<0的输出置为0，就是一个去噪音，稀疏矩阵的过程。而且在训练过程中，这种稀疏性是动态调节的，网络会自动调整稀疏比例，保证矩阵有最优的有效特征。

但是ReLU 强制将x<0部分的输出置为0（置为0就是屏蔽该特征），可能会导致模型无法学习到有效特征，所以如果学习率设置的太大，就可能会导致网络的大部分神经元处于‘dead’状态，所以使用ReLU的网络，学习率不能设置太大。

与sigmoid类似，ReLU的输出均值也大于0，所以偏移现象和神经元死亡共同影响网络的收敛性。

**ReLU作为激活函数的特点：**

- 相比Sigmoid和tanh，ReLU摒弃了复杂的计算，提高了运算速度。
- 解决了梯度消失问题，收敛速度快于Sigmoid和tanh函数，但要防范ReLU的梯度爆炸
- 容易得到更好的模型，但也要防止训练中出现模型‘Dead’情况。



#### Leaky ReLU, PReLU（Parametric Relu）, RReLU（Random ReLU）

为了防止模型的‘Dead’情况，后人将x<0部分并没有直接置为0，而是给了一个很小的负数梯度值![[公式]](https://www.zhihu.com/equation?tex=%5Calpha)。

**Leaky ReLU**

$f(x)=\left\{\begin{array}{ll}x, & \text { if } x \geq 0 \\ \alpha x, & \text { if } x<0\end{array}\right.$

$\alpha$ 为常数，一般设置 0.01。这个函数通常比 Relu 激活函数效果要好，但是效果不是很稳定，所以在实际中 Leaky ReLu 使用的并不多。

**PRelu（参数化修正线性单元）** 中的![[公式]](https://www.zhihu.com/equation?tex=%5Calpha+)作为一个可学习的参数，会在训练的过程中进行更新。

**RReLU（随机纠正线性单元）**也是Leaky ReLU的一个变体。在RReLU中，负值的斜率在训练中是随机的，在之后的测试中就变成了固定的了。RReLU的亮点在于，在训练环节中，aji是从一个均匀的分布U(I,u)中随机抽取的数值。



#### ELU (Exponential Linear Units) 函数

$f(x)=\left\{\begin{array}{ll}x, & \text { if } x>0 \\ \alpha\left(e^{x}-1\right), & \text { otherwise }\end{array}\right.$

![image-20210326221045623](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210326221045623.png)

右侧线性部分使得ELU能缓解梯度消失，而左侧软饱和能让对ELU对输入变化或噪声更鲁棒。ELU的输出均值接近于0，所以收敛速度更快。

不会有Dead ReLU问题 输出的均值接近0，zero-centered

它的一个小问题在于计算量稍大。类似于Leaky ReLU，理论上虽然好于ReLU，但在实际使用中目前并没有好的证据ELU总是优于ReLU。







参考图像：



![image-20210326220447212](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210326220447212.png)

![image-20210326220725637](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210326220725637.png)

参考链接：

[知乎](https://zhuanlan.zhihu.com/p/73214810)

[简书](jianshu.com/p/dc4e53fc73a0)

[CSDN](https://blog.csdn.net/tyhj_sf/article/details/79932893)