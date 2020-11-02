---

layout: post
title: '区间问题'
date: 2020-11-02
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Interval problem



#### 重叠区间的合并

一个区间可以表示为`[start,end]`，为了清晰起见，我们选择按`start`排序。

<img src="C:%5CUsers%5C74116%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201102155840219.png" alt="image-20201102155840219" style="zoom:50%;" />

显然，对于几个相交区间合并后的结果区间`x`，`x.start`一定是这些相交区间中`start`最小的，`x.end`一定是这些相交区间中`end`最大的。

<img src="https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20201102155917522.png" alt="image-20201102155917522" style="zoom: 50%;" />

相关题目：
[56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)



#### 区间调度问题（贪心算法）

给你很多形如`[start,end]`的闭区间，请你设计一个算法，**算出这些区间中最多有几个互不相交的区间**。

正确思路：

可以分为以下三步：

1. 从区间集合 intvs 中选择一个区间 x，这个 x 是在当前所有区间中**结束最早的**（end 最小）。
2. 把所有与 x 区间相交的区间从区间集合 intvs 中删除。
3. 重复步骤 1 和 2，直到 intvs 为空为止。之前选出的那些 x 就是最大不相交子集。

具体算法：

1. 按每个区间的`end`数值升序排序

2. 与 x 相交的区间必然会与 x 的`end`相交；如果一个区间不想与 x 的`end`相交，它的`start`必须要大于（或等于）x 的`end`

   <img src="C:%5CUsers%5C74116%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201102160259218.png" alt="image-20201102160259218" style="zoom:50%;" />

相关题目：
[435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)
[452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

参考资料：

[CNBLOG](https://www.cnblogs.com/cmmdc/p/7190116.html)



#### 两组区间的交集

**解决区间问题的思路一般是先排序，以便操作**，可以用两个索引指针在`A`和`B`中游走，把交集找出来

首先，**对于两个区间**，我们用`[a1,a2]`和`[b1,b2]`表示在`A`和`B`中的两个区间，那么什么情况下这两个区间**没有交集**呢：

```
if b2 < a1 or a2 < b1:
    [a1,a2] 和 [b1,b2] 无交集	
```

则存在交集的为：

```
# 不等号取反，or 也要变成 and
if b2 >= a1 and a2 >= b1:
    [a1,a2] 和 [b1,b2] 存在交集
```

寻找交集的共同点

<img src="C:%5CUsers%5C74116%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201102170228880.png" alt="image-20201102170228880" style="zoom:50%;" />

指针 $i,j$ 的前进

```
while i < len(A) and j < len(B):
    # ...
    if b2 < a2:
        j += 1
    else:
        i += 1
```

相关题目：

[986. 区间列表的交集](https://leetcode-cn.com/problems/interval-list-intersections/)



#### 相关题目：

[1288. 删除被覆盖区间](https://leetcode-cn.com/problems/remove-covered-intervals/)

#### 参考资料：

[labuladong](https://labuladong.gitbook.io/algo/di-ling-zhang-bi-du-xi-lie-qing-an-shun-xu-yue-du/qu-jian-wen-ti-he-ji)
[wechat](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247484493&idx=1&sn=1615b8a875b770f25875dab54b7f0f6f&chksm=9bd7fa45aca07353a347b7267aaab78b81502cf7eb60d0510ca9109d3b9c0a1d9dda10d99f50&scene=21#wechat_redirect)