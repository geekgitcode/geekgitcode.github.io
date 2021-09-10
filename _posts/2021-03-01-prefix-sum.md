---

layout: post
title: '前缀和'
date: 2021-03-01
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Prefix sum



什么是前缀和？前缀和是一个数组的某项下标之前(包括此项元素)的所有数组元素的和。 

前缀和可以在O(n)时间统计和修改，在O(1)时间内查询统计任意区间之和；

#### 一维前缀和 ：

对于一个给定的数列 A, 它的前缀和数列S 是通过递推能求出来得

![image-20210301102914796](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210301102914796.png)

部分和,即数列A中某个下标区间内的数的和,可以表示为前缀和相减的形式:

![image-20210301102922883](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210301102922883.png)

```
//a数组是数字数组，如果从a[0]开始则为a[i-1]，如果从1开始为a[i]
//sum[i] 表示从a[0]+...+a[i]; i从1->n
//sum[L,R]= s[R] - s[L-1]


for(int i = 1; i <= n; ++i) 
	sum[i] = sum[i - 1] + a[i-1];　　//O(n)

//所以每次我们询问区间[L,R] 的和,只需要计算s[R+1] - s[L] 就可以了. 
//L和R注意从0开始还是从1开始
//从0开始s[R+1] - s[L], 可以先R++,L++
//从1开始s[R] - s[L-1]
while(m--)　　　　　　　　//O(m)
{
	int L, R; 
	scanf("%d%d", &L, &R);
	printf("%d\n", sum[R] - sum[L - 1]);
}
```

#### 二维前缀和：

![image-20210301104336686](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210301104336686.png)



```
mat从0开始
sum从1开始
//求原点到i，j的矩形和
//sum[i][j] = sum[i-1][j]+sum[i][j-1]-sum[i-1][j-1]+mat[i-1][j-1]
for i in range(1, m + 1):
    for j in range(1, n + 1):
        sum[i][j] = mat[i - 1][j - 1] + sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1]
```

![image-20210301111412015](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210301111412015.png)



![image-20210301111355155](https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20210301111355155.png)

```
求点(x1,y1)到(x2,y2)的矩形和 sum从1开始
//特殊记忆方法如下
//sum[i][j] = sum[i-1][j]+sum[i][j-1]-sum[i-1][j-1]+mat[i-1][j-1]
//mat[i-1][j-1] = sum[i][j]-sum[i-1][j]-sum[i][j-1]+sum[i-1][j-1]
//将(x1,y1),(x2,y2)代入上式 

sum[x2][y2]-sum[x1-1][y2]-sum[x2][y1-1]+sum[x1-1][y1-1]
```

#### 一些注意的点：

一般设置sum数组都是从1开始作为下标

但是查询区间和的时候我们通常是从0开始的，所以在给出区间查询时要注意+1，保证数组下标不会越界。

#### 题目：

[303. 区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)

[304. 二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)

#### 参考链接：

[CSDN](https://www.cnblogs.com/-Ackerman/p/11162651.html)

[知乎](https://zhuanlan.zhihu.com/p/108064211)

[](https://segmentfault.com/a/1190000022512260)