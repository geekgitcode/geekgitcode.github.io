---
layout: post
title: 'Exercise-1'
date: 2020-09-19
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/ml-andrewng.png'
tags: ML-AndrewNg
---

> Exercise-1 Linear Regression



### 练习1.1：单变量线性回归 （Linear Regression with One Variable）

在进入练习一之前，请确保你已经了解的文章：所定义的各变量维度

练习1用到的公式：

代价函数    $J \left(\theta_0, \theta_1 \right) = \frac1{2m}\sum\limits_{i=1}^m\left( h_\theta(x^{(i)})-y^{(i)}\right)^2$

代价函数的偏导数    $\frac\partial{\partial\theta}J(\theta_0,\theta_1)=\frac1m\sum\limits_{i=1}^m\left( \left(h_\theta(x^{(i)})-y^{(i)}\right)\cdot x^{(i)}\right)$

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



### 练习1.2：多变量线性回归 （Linear Regression with Multiple Variable）

和练习一并无较大差异，注意Feature Normalization即可

```python
# 特征归一化
def featureNormalize(X):
    # axis = 0：压缩行，对各列求均值，返回 1*n 矩阵
    mu = np.mean(X, axis=0)
    sigma = np.std(X, axis=0)
    x_norm = np.divide(X - mu, sigma)
    return x_norm, mu, sigma
```



全部代码：

```python
import numpy as np
import matplotlib.pyplot as plt

# 让numpy不用科学计数法显示
np.set_printoptions(formatter={'all': lambda x: str(x)}, threshold=100)

data = np.loadtxt('ex1data2.txt', delimiter=',')
X = data[:, :-1]  # (47,2)
y = data[:, -1].reshape(-1, 1)  # (47,1)


# 特征归一化
def featureNormalize(X):
    # axis = 0：压缩行，对各列求均值，返回 1*n 矩阵
    mu = np.mean(X, axis=0)
    sigma = np.std(X, axis=0)
    x_norm = np.divide(X - mu, sigma)
    return x_norm, mu, sigma


X, mu, sigma = featureNormalize(X)
X = np.insert(X, 0, np.ones(1), axis=1)  # 在x0插入一列全1
theta = np.zeros((X.shape[1], 1), dtype=np.float)  # theta个数为X的特征值个数 (3,1)


def computerCost(X, y, theta):
    m = X.shape[0]
    h = np.dot(X, theta)  # (m,n) (n,1)  ~ (m,1)
    J = 1 / (2 * m) * np.sum(np.power(h - y, 2))  # (m,1)进行power还是(m,1) sum对列向量求和返回值
    return J

    # 65591548106.45744


def gradientDescent(X, y, theta, alpha, iters):
    m = X.shape[0]
    cost = np.zeros(iters)
    for i in range(iters):
        h = np.dot(X, theta)  # (m,1)
        # (n,1) ~ (n,1) - (n,m)(m,1)
        theta = theta - (alpha / m) * np.dot(np.transpose(X), h - y)
        cost[i] = computerCost(X, y, theta)
    return theta, cost


alpha = 0.01
iters = 400
finalTheta, cost = gradientDescent(X, y, theta, alpha, iters)
plt.plot(np.arange(len(cost)), cost, '-b', lw=2)
plt.xlabel('Number of iterations')
plt.ylabel('Cost J')
plt.show()

print('Theta computed from gradient descent: ', finalTheta)
'''
[[334302.06399327697]
 [99411.4494735893]
 [3267.012854065695]]
'''

X_test = np.array([1650, 3])
X_test = np.divide(X_test - mu, sigma)  # 归一化
X_test = np.hstack((1, X_test))
price = np.dot(X_test, finalTheta)
print('Predicted price of a 1650 sq-ft, 3 br house (using gradient descent): ', price)
'''
[289221.5473712181]
'''
```

Convergence of gradient descent with an appropriate learning rate：

<img src="C:%5CUsers%5C74116%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200918183959658.png" alt="image-20200918183959658" style="zoom:67%;" />

### 练习1.3：正规方程 （Normal Equation）

```python
import numpy as np

data = np.loadtxt('ex1data2.txt', delimiter=',')
X = data[:, :-1]  # (47,2)
y = data[:, -1].reshape(-1, 1)  # (47,1)
X = np.insert(X, 0, np.ones(1), axis=1)  # 在x0插入一列全1


def normalEqn(X, y):
    theta = (np.dot(np.linalg.inv(np.dot(np.transpose(X), X)), np.transpose(X))).dot(y)
    return theta


theta = normalEqn(X, y)
print('Theta computed from the normal equations: ', theta)
'''
[[89597.9095428 ]
 [  139.21067402]
 [-8738.01911233]]
'''

# Estimate the price of a 1650 sq-ft, 3 br house
X_test = np.array([1, 1650, 3])
price = X_test.dot(theta)
print('Predicted price of a 1650 sq-ft, 3 br house (using normal equations): ', price)
'''
[293081.4643349]
'''
```

与递归下降相比，正规方程的误差在可接受范围内

$\theta ={{\left( {X^{T}}X \right)}^{-1}}{X^{T}}y$ 的推导过程：

$J \left(\theta\right) = \frac1{2m}\sum\limits_{i=1}^m\left( h_\theta(x^{(i)})-y^{(i)}\right)^2$
其中：$h_\theta(x)=\theta^Tx=\theta_0x_0+\theta_1x_1+\cdots+\theta_nx_n$

将向量表达形式转为矩阵表达形式，则有$J(\theta)=\frac{1}{2}\left( X\theta -y\right)^2$ ，其中$X$为$m$行$n$列的矩阵（$m$为样本个数，$n$为特征个数），$\theta$为$n$行1列的矩阵，$y$为$m$行1列的矩阵，对$J(\theta )$进行如下变换

$J(\theta )=\frac{1}{2}\left( X\theta -y\right)^T\left( X\theta -y \right)$

$=\frac{1}{2}\left( \theta^TX^T-y^T \right)\left(X\theta -y \right)$
    
$=\frac{1}{2}\left(\theta^TX^TX\theta - \theta^TX^Ty-y^TX\theta -y^Ty\right)$

接下来对$J(\theta )$偏导，需要用到以下几个矩阵的求导法则:

$\frac{dAB}{dB}=A^T$ 

$\frac{dX^TAX}{dX}=2AX$                            

所以有:

$\frac{\partial J\left( \theta  \right)}{\partial \theta }=\frac{1}{2}\left(2X^TX\theta -X^Ty -(y^TX)^{T}-0 \right)$

​          $=\frac{1}{2}\left(2X^TX\theta -X^Ty -X^Ty -0 \right)  $
​    
​          $=X^TX\theta -X^Ty$

 令$\frac{\partial J\left( \theta  \right)}{\partial \theta }=0$,

则有$\theta =\left( X^TX \right)^{-1}X^Ty$