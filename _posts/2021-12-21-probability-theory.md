---
layout: post
title: '概率论相关知识'
date: 2021-12-01
author: downeyking
cover: 'https://i.loli.net/2020/01/10/oWigTw6IzbGr9cf.png'
tags: DL

---

> 概率论相关知识 持续更新~
#### **独立同分布：**

随机过程中，任何时刻的取值都为随机变量，如果这些随机变量服从同一分布，并且互相独立，那么这些随机变量是独立同分布，随机变量X1，X2的取值满足独立，且属于同一分布序列，X1的取值不影响X2的取值，拥有同样的概率分布函数

#### **贝叶斯 执果索因** 

在某件大事件发生了的前提下 通过我们对小事件的观测 去判断造成大事件发生的原因的可能性

贝叶斯公式是建立在条件概率的基础上寻找事件发生的原因（即大事件A已经发生的条件下，分割中的小事件Bi的概率），设B1, B2,...是样本空间Ω的一个划分，则对任一事件A（P(A)>0), 有

![image-20220314162437534](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20220314162437534.png)

#### **朴素贝叶斯分类：**

理论上，概率模型分类器是一个条件概率模型：



![[公式]](https://www.zhihu.com/equation?tex=p%28C%7CF1%2C...%2CFn%29)



独立变量C有若干类别，条件依赖于若干特征变量,但问题在于如果特征数量n的维度较大或者每个特征能取大量值时，基于概率模型列出概率表变得不现实。所以我们修改这个模型使之变得可行。 根据贝叶斯公式有以下式子：



![[公式]](https://www.zhihu.com/equation?tex=p%28C%7CF1%2C...%2CFn%29%3D+%5Cfrac%7Bp%28C%29%2Ap%28F1%2C...Fn%7CC%29%7D%7Bp%28F1%2C...%2CFn%29%7D)



或者，这样表达比较简洁明了：



![[公式]](https://www.zhihu.com/equation?tex=p%28%E7%B1%BB%E5%88%AB%7C%E7%89%B9%E5%BE%81%29%3D%5Cfrac%7Bp%28%E7%B1%BB%E5%88%AB%29%2Ap%28%E7%B1%BB%E5%88%AB%7C%E7%89%B9%E5%BE%81%29%7D%7Bp%28%E7%89%B9%E5%BE%81%29%7D)



其中， ![[公式]](https://www.zhihu.com/equation?tex=p%28C%29) 为先验概率， ![[公式]](https://www.zhihu.com/equation?tex=p%28C%7CF1%2C...%2CFn%29) 为后验概率；可以这么理解，再不知道需要预测的样本任何特征的时候，先判断该样本为某个类别的概率为 ![[公式]](https://www.zhihu.com/equation?tex=p%28C%29) 为，再知道样本的特征之后，乘上 ![[公式]](https://www.zhihu.com/equation?tex=%5Cfrac%7Bp%28F1%2C...Fn%7CC%29%7D%7Bp%28F1%2C...Fn%29%7D) 之后，得到该样本再知道 ![[公式]](https://www.zhihu.com/equation?tex=F1%3Df1%2C...%2CFn%3Dfn) 之后，样本属于这个类别的条件概率。

这个乘上去的因子可能是起到促进的作用（当该因子大于1），也可能起到抑制的作用（当该因子小于1）。这个比较容易理解，比如没有任何信息的时候，可以判断一个官为贪官的概率为0.5，再知道该官员财产大于一千万后，则根据常理判断该官员为贪官的概率为0.8。

实际中，我们只关心分式中的分子部分 ![[公式]](https://www.zhihu.com/equation?tex=p%28C%29%2Ap%28F1%2C...Fn%7CC%29) ，因为分母不依赖于C,而且特征的值也是给定的，于是分母可以认为是一个常数。这样分子就等价于[联合分布](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%E8%81%94%E5%90%88%E5%88%86%E5%B8%83%22%20%5Co%20%22%E8%81%94%E5%90%88%E5%88%86%E5%B8%83)模型。

![[公式]](https://www.zhihu.com/equation?tex=p%28C%2CF1%2C...Fn%29)

![img](https://pic1.zhimg.com/80/v2-ac5af445b2337e006ab2a8f0016dcb38_1440w.jpg)

现在，“朴素”的条件独立假设开始发挥作用了：假设每个特征,对于其他特征,是独立的，即特征之间相互独立，就有：



![[公式]](https://www.zhihu.com/equation?tex=p%28Fi%7CC%2CFj%29%3Dp%28Fi%7CC%29)



这里还要再解释一下为什么要假设特征之间相互独立。

我们这么想，假如没有这个假设，在数据量很大的情况下，那么我们对右边这些概率的估计其实是不可做的，这么说，假设一个分类器有4个特征，每个特征有10个特征值，则这四个特征的联合概率分布是4维的，可能的情况就有 ![[公式]](https://www.zhihu.com/equation?tex=10%5E%7B4%7D%3D10000) 种。

计算机扫描统计还可以，但是现实生活中，往往有非常多的特征，每一个特征的取值也是非常之多，那么通过统计来估计后面概率的值，变得几乎不可做，这也是为什么需要假设特征之间独立的原因，朴素贝叶斯法对条件概率分布做了条件独立性的假设，由于这是一个较强的假设，朴素贝叶斯也由此得名！这一假设使得朴素贝叶斯法变得简单，但有时会牺牲一定的分类准确率。

有了特征相互独立的条件以后，对于,联合分布模型可表达为：

![img](https://pic1.zhimg.com/80/v2-ac5af445b2337e006ab2a8f0016dcb38_1440w.jpg)

这就意味着，变量C的条件分布可以表达为：

![img](https://pic2.zhimg.com/80/v2-b167636123072b605b22e2e7450fe149_1440w.jpg)

其中，Z只依赖 ![[公式]](https://www.zhihu.com/equation?tex=F1%2C...%2CFn) ,当特征变量已知时Z是个常数。

至此，我们我们可以从概率模型中构造分类器，朴素贝叶斯[分类器](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/w/index.php%3Ftitle%3D%E5%88%86%E7%B1%BB%E5%99%A8%26action%3Dedit%26redlink%3D1%22%20%5Co%20%22%E5%88%86%E7%B1%BB%E5%99%A8%EF%BC%88%E9%A1%B5%E9%9D%A2%E4%B8%8D%E5%AD%98%E5%9C%A8%EF%BC%89)包括了这种模型和相应的决策规则。一个普通的规则就是选出最有可能的那个：这就是大家熟知的[最大后验概率](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%E6%9C%80%E5%A4%A7%E5%90%8E%E9%AA%8C%E6%A6%82%E7%8E%87%22%20%5Co%20%22%E6%9C%80%E5%A4%A7%E5%90%8E%E9%AA%8C%E6%A6%82%E7%8E%87)（MAP）决策准则。

相应的分类器便是如下定义的公式：

![img](https://pic4.zhimg.com/80/v2-8db89533a0915a63cd9f320c1c06b73f_1440w.jpg)

**当特征值为离散型时**：

类的先验概率可以通过训练集的各类样本的出现次数来估计，例如：

类的先验概率 ![[公式]](https://www.zhihu.com/equation?tex=p%28C%3Dc1%29%3D%5Cfrac%7B1%7D%7B%E6%A0%B7%E6%9C%AC%E6%80%BB%E6%95%B0%7D)

类条件概率= ![[公式]](https://www.zhihu.com/equation?tex=p%28Fi%3Dfi%7CC%3Dc1%29%3D%5Cfrac%7BFi%3Dfi%E7%9A%84%E6%A0%B7%E6%9C%AC%E6%80%BB%E9%87%8F%7D%7B%E6%A0%B7%E6%9C%AC%E6%80%BB%E6%95%B0%7D)

即可求得类的条件概率，最后比较各个类别的概率值的大小判断该测试样本应该属于哪个类别。



**当特征值为连续型时**：

通常的假设这些连续数值为高斯分布。例如，假设训练集中某个连续特征 ![[公式]](https://www.zhihu.com/equation?tex=x) 。首先我们对数据类别分类，然后计算每个类别中的 ![[公式]](https://www.zhihu.com/equation?tex=x) 均值和方差。令 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmu+_%7Bc%7D) 表示为 ![[公式]](https://www.zhihu.com/equation?tex=x) 在**c**类上的均值， ![[公式]](https://www.zhihu.com/equation?tex=%5Csigma+_%7Bc%7D%5E%7B2%7D) 表示为 ![[公式]](https://www.zhihu.com/equation?tex=x) 在**c**类上的方差。在给定类中某个值的概率 ![[公式]](https://www.zhihu.com/equation?tex=p%3D%EF%BC%88x%3Dv%7Cc%EF%BC%89)，可以通过将 ![[公式]](https://www.zhihu.com/equation?tex=v) 表示为均值为 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmu+_%7Bc%7D) 方差为 ![[公式]](https://www.zhihu.com/equation?tex=%5Csigma+_%7Bc%7D%5E%7B2%7D) 的正态分布计算出来。如下，

![img](https://pic2.zhimg.com/80/v2-03c4c2b143f7fd32103937b8e01d6445_1440w.jpg)

处理连续数值问题的另一种常用的技术是通过离散化连续数值的方法，通常，当训练样本数量较少或者是精确的分布已知时，通过概率分布的方法是一种更好的选择。

在大量样本的情形下离散化的方法表现更优，因为大量的样本可以学习到数据的分布。由于朴素贝叶斯是一种典型的用到大量样本的方法（越大计算量的模型可以产生越高的分类精确度），所以朴素贝叶斯方法都用到离散化方法，而不是概率分布估计的方法。

#### **似然函数：**

概率用于在已知一些参数的情况下，预测接下来的观测所得到的结果，而似然性 则是用于在已知某些观测所得到的结果时，对有关事物的性质的参数进行估计。在这种意义上，似然函数可以理解为条件概率的逆反。

似然函数是给定联合样本值![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7Bx%7D)下关于(未知)参数![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta) 的函数：![[公式]](https://www.zhihu.com/equation?tex=L%28%5Ctheta+%7C+%5Ctextbf%7Bx%7D%29+%3D+f%28%5Ctextbf%7Bx%7D+%7C+%5Ctheta%29)

这里的小![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7Bx%7D)是指联合样本随机变量![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7BX%7D)取到的值，即![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7BX%7D+%3D+%5Ctextbf%7Bx%7D)；

这里的![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta)是指未知参数，它属于参数空间；

这里的![[公式]](https://www.zhihu.com/equation?tex=f%28%5Ctextbf%7Bx%7D%7C%5Ctheta%29)是一个密度函数，特别地，它表示(给定)![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta)下关于联合样本值![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7Bx%7D)的联合密度函数。

所以从定义上，似然函数和密度函数是完全不同的两个**数学对象**：前者是关于![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta)的函数，后者是关于![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7Bx%7D)的函数。所以这里的等号![[公式]](https://www.zhihu.com/equation?tex=%3D) 理解为**函数值形式**的相等，而不是两个函数本身是同一函数(根据函数相等的定义，函数相等当且仅当定义域相等并且对应关系相等)。

**概率**(密度)表达给定![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta)下样本随机向量![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7BX%7D+%3D+%5Ctextbf%7Bx%7D)的**可能性**，而**似然**表达了给定样本![[公式]](https://www.zhihu.com/equation?tex=%5Ctextbf%7BX%7D+%3D+%5Ctextbf%7Bx%7D)下参数![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_1)(相对于另外的参数![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_2))为真实值的**可能性**。我们总是对随机变量的取值谈**概率**，而在非贝叶斯统计的角度下，参数是一个实数而非随机变量，所以我们一般不谈一个参数的**概率**

极大似然是**频率学派的参数估计方法**，设总体分布为 ![[公式]](https://www.zhihu.com/equation?tex=f%28X%3B%5Ctheta%29) ， ![[公式]](https://www.zhihu.com/equation?tex=x_1%2C%5Cdots%2Cx_n) 是从总体分布中抽出的样本， 那么样本![[公式]](https://www.zhihu.com/equation?tex=%28x_1%2C%5Cdots%2Cx_n%29)的联合分布为： ![[公式]](https://www.zhihu.com/equation?tex=L%28x_1%2Cx_2%2C%5Cdots%2Cx_n%3B%5Ctheta%29%3Df%28x_1%3B%5Ctheta%29+f%28x_2%3B%5Ctheta%29+%5Ccdots+f%28x_n%3B%5Ctheta%29)

当固定 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta) 时，看作是 ![[公式]](https://www.zhihu.com/equation?tex=x_1%2C%5Cdots%2Cx_n) 的函数时，L是一个概率密度函数。

当固定 ![[公式]](https://www.zhihu.com/equation?tex=x_1%2C%5Cdots%2Cx_n) 时， 把 L 看作是 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta) 的函数，由于 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta) 有一定的值，但是未知，并非随机变量（频率学派观点），不能叫做概率，而叫做似然（likelihood）。

使得likelihood最大的那个点记为：

![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta%5E%2A%3D+argmax+L%28x_1%2C%5Cdots%2Cx_n%3B%5Ctheta%29)

并将其并作为 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta) 的估计值，在已有的样本 ![[公式]](https://www.zhihu.com/equation?tex=x_1%2C%5Cdots%2Cx_n) 条件下， ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta%5E%2A) 就叫做 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta) 的**极大似然估计**。



判定模型：学习边缘分布得到条件概率

生成模型：学习联合分布得到概率

二者目的都是在使后验概率最大化，判别式是直接对后验概率建模，但是生成模型通过贝叶斯定理这一“桥梁”使问题转化为求联合概率



生成模型：
优点：
1）生成给出的是联合分布![[公式]](https://www.zhihu.com/equation?tex=P%28%5Ctilde%7Bx%7D%2C%5Ctilde%7Bc%7D%29+)，不仅能够由联合分布计算条件分布![[公式]](https://www.zhihu.com/equation?tex=P%28%5Ctilde%7Bc%7D%7C%5Ctilde%7Bx%7D%29)（反之则不行），还可以给出其他信息，比如可以使用![[公式]](https://www.zhihu.com/equation?tex=P%28%5Ctilde%7Bx%7D+%29%3D%5Csum_%7Bi%3D1%7D%5E%7Bk%7D%7BP%28%5Ctilde%7Bx%7D%7C%5Ctilde%7Bc%7D_%7Bi%7D%29%7D+)![[公式]](https://www.zhihu.com/equation?tex=%2AP%28%5Ctilde%7Bc_i%7D+%29)来计算边缘分布![[公式]](https://www.zhihu.com/equation?tex=P%28%5Ctilde%7Bx%7D+%29)。如果一个输入样本的边缘分布![[公式]](https://www.zhihu.com/equation?tex=P%28%5Ctilde%7Bx%7D+%29)很小的话，那么可以认为学习出的这个模型可能不太适合对这个样本进行分类，分类效果可能会不好，这也是所谓的*outlier detection。*
2）生成模型收敛速度比较快，即当样本数量较多时，生成模型能更快地收敛于真实模型。
3）生成模型能够应付存在隐变量的情况，比如混合高斯模型就是含有隐变量的生成方法。

缺点：
1）天下没有免费午餐，联合分布是能提供更多的信息，但也需要更多的样本和更多计算，尤其是为了更准确估计类别条件分布，需要增加样本的数目，而且类别条件概率的许多信息是我们做分类用不到，因而如果我们只需要做分类任务，就浪费了计算资源。
2）另外，实践中多数情况下判别模型效果更好。

判别模型：
优点：
1）与生成模型缺点对应，首先是节省计算资源，另外，需要的样本数量也少于生成模型。
2）准确率往往较生成模型高。
3）由于直接学习![[公式]](https://www.zhihu.com/equation?tex=P%28%5Ctilde%7Bc%7D%7C%5Ctilde%7Bx%7D+%29)，而不需要求解类别条件概率，所以允许我们对输入进行抽象（比如降维、构造等），从而能够简化学习问题。

缺点：
1）是没有生成模型的上述优点。



参考：

[知乎](https://zhuanlan.zhihu.com/p/37575364)