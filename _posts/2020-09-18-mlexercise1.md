---
layout: post
title: 'Exercise-1'
date: 2020-09-18
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/ml-andrewng.png'
tags: ML-AndrewNg
---

> Exercise-1 Linear Regression with One Variable



### 练习一：单变量线性回归 （Linear Regression with One Variable）

在进入练习一之前，请确保你已经了解的文章：所定义的各变量维度

练习1用到的公式：

代价函数    $J \left( \theta_0, \theta_1 \right) = \frac{1}{2m}\sum\limits_{i=1}^m \left( h_{\theta}(x^{(i)})-y^{(i)} \right)^{2}$

代价函数的偏导数    $\frac\partial{\partial\theta}J(\theta_0,\theta_1)=\frac{1}{m}\sum\limits_{i=1}^{m}{\left( \left( h_\theta(x^{(i)})-y^{(i)} \right)\cdot x^{(i)} \right)}$

使用梯度下降求代价函数的最小值

**Repeat {**
​           	   $\theta:=\theta-a\frac1m\sum\limits_{i=1}^m\left(\left(h_\theta(x^{(i)})-y^{(i)}\right)\cdot x^{(i)}\right)$
​               **}**

代码：

1.导入我们需要的库

```python
import numpy as np
import matplotlib.pyplot as plt
```

2.将初始数据可视化

```python
# "------------------plot data--------------------"
data = np.loadtxt('ex1data1.txt', delimiter=',', usecols=(0, 1))
X = data[:, :-1]     #  (97,1)
y = data[:, -1].reshape(-1,1)    #  (97,1)
plt.title("MarkerSize")
plt.xlabel("Profit in $10,000s")
plt.ylabel("Population of City in 10,000s")
plt.plot(X, y, 'rx')
plt.show()
```

<img src="https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20200918114351229.png" alt="image-20200918114351229" style="zoom:65%;" />

3.数据预处理

```python
# "------------------data preprocess-----------------"
data = np.insert(data, 0, np.ones(1), axis=1)  # 在x0插入一列全1
X = data[:, :-1]
theta = np.zeros((X.shape[1], 1), dtype=np.float)  # theta个数为X的特征值个数 (2,1)
```

4.计算代价$J$  任选一种方法即可

```python
# "------------------computer cost--------------------"
# 方法一 返回(1,1)向量
def computerCost(X, y, theta):
    m = X.shape[0]
    h = np.dot(X, theta)  # (m,n) (n,1)  ~ (m,1)
    J = 1 / (2 * m) * np.dot(np.transpose(h - y), h - y)  # (1,m) (m,1) ~ (1,1)
    return J
```

```python
# 方法二 返回值
def computerCost(X, y, theta):
    m = X.shape[0]
    h = np.dot(X, theta)  # (m,n) (n,1)  ~ (m,1)
    J = 1 / (2 * m) * np.sum(np.power(h - y, 2))  # (m,1)进行power还是(m,1) sum对列向量求和返回值
    return J
```

```python
# 测试computerCost函数
print(computerCost(X, y, theta)) 
'''
方法一返回[[32.07273388]]
方法二返回32.072733877455676
'''
```

5.梯度下降优化 $\theta$

```python
# "------------------gradient descent--------------------"
def gradientDescent(X, y, theta, alpha, iters):
    m = X.shape[0]
    cost = np.zeros(iters)
    for i in range(iters):
        h = np.dot(X, theta)  # (m,1)
        # (n,1) ~ (n,1) - (n,m)(m,1)
        theta = theta - (alpha / m) * np.dot(np.transpose(X), h - y)
        cost[i] = computerCost(X, y, theta)  # 保存每次损失值J便于后续画图
    return theta, cost
```

```python
# 测试gradientDescent函数
alpha = 0.01
iters = 1500
finalTheta, cost = gradientDescent(X, y, theta, alpha, iters)
print(finalTheta)
'''
[[-3.63029144]
 [ 1.16636235]]
'''
```

6.可视化结果

```python
# "------------------visualize answer-----------------"
h = np.dot(X, finalTheta)
fig1 = plt.subplot(211)
fig1.set_title("MarkerSize")
fig1.set_xlabel("Profit in $10,000s")
fig1.set_ylabel("Population of City in 10,000s")
plt.plot(X[:, 1:], y, 'rx')
plt.plot(X[:, 1:], h)

fig2 = plt.subplot(212)
fig2.set_xlabel("epoch")
fig2.set_ylabel("cost")
plt.plot(np.arange(len(cost)), cost)

plt.show()
```

<img src="https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20200918122638172.png" alt="image-20200918122638172" style="zoom:65%;" />