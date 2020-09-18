---
layout: post
title: '预先定义'
date: 2020-09-18
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/ml-andrewng.png'
tags: ML-AndrewNg
---

> Pre-defined



### 第零章 

在后续的文章中，我们将用到如下预先定义：

1. 将 $\theta$ 定义为$(n, 1)$的向量，其中 $n$ 代表样本 $x^{(i)}$ 的特征的个数

2. 样本$X$为$(m,n)$的矩阵，其中 $m$ 代表样本个数，$n$ 代表样本特征个数

   $x_j^{(i)}$代表第 $i$ 个样本的第 $j$ 个特征

3. 样本 $y$ 为$(m,1)$的向量，$y^i$代表第 $i$ 个样本的输出，为值，标量 

4. 假设 $(hypothesis)$ 用 $h$ 表示

   $h_\theta{(x)^{(i)}}=\theta^Tx^{(i)}=\theta_0x_0+\theta_1x_1+\cdots+\theta_nx_n$  代表某个样本的假设值 / 预测值

   $\theta$ 为$(n,1)$向量，$x^{(i)}$为$(n,1)$向量

5. 对于向量化整体运算：

   $h_\theta(x)=X\theta$ 

   $(m,1) \sim (m,n)(n,1)$

6. $np.sum()$ 一般用于对列向量求和

7. $J\left( {\theta_{0}},{\theta_{1}}...{\theta_{n}} \right)$ 返回值，标量

8. 对于$J\left( {\theta_0},{\theta_1} \cdots {\theta_n} \right)=\frac1{2m}\sum\limits_{i=1}^m\left( h_\theta \left(x^{\left( i\right)} \right)-y^{\left( i \right)} \right)^2$, 由7知 $J$ 返回的应该是个标量，所以在使用向量化整体计算时需要注意几个点:

   第一种计算方式 $J = \frac 1{2m} * np.sum(np.power(h_\theta(x)-y),2)$

   $h_\theta(x)$ 和 $y$ 都是$(m,1)$向量，所以二者做幂运算也是$(m,1)$向量，使用 $np.sum()$对列向量求和返回值

   第二种计算方式是 $J = \frac 1{2m} * np.dot((h_\theta(x)-y)^T,(h_\theta(x)-y))$

   $(h_\theta(x)-y)^T$是$(1,m)$向量，$(h_\theta(x)-y)$是$(m,1)$向量，则 $(1,m)(m,1) \sim (1,1)$，返回$(1,1)$向量

9. 在对数据进行预处理时，我们一般对$X$插入一列
$$
x_0 = 
\begin{bmatrix}   
{1}  \\   
{1}  \\   
{\vdots}  \\   
{1}  \\
\end{bmatrix}
$$
​		用来匹配 $\theta_0$



   

   




