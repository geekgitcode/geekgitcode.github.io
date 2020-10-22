---
layout: post
title: '二分搜索'
date: 2020-10-20
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Binary Search



二分搜索，经常用于查找一个数，寻找左侧边界，寻找右侧边界等

时间复杂度   $O\left(logN\right)$

#### 普通二分搜索框架：

```c++
int search(vector<int>& nums, int target) {
        if(nums.size()==0)
            return -1;
        int left = 0;
        int right = nums.size()-1;
        while(left<=right){
            int mid = left + (right-left)/2;  //防止溢出
            if(nums[mid]<target)
                left = mid+1;
            else if(nums[mid]>target)
                right = mid-1;
            else
                return mid;
        }
        return -1;
    }
```

#### 寻找左侧边界的二分搜索：

```c++
int search_left_bound(vector<int>& nums, int target) {
        if(nums.size()==0)
            return -1;
        int left = 0;
        int right = nums.size()-1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]<target)
                left = mid+1;
            else if(nums[mid]>target)
                right = mid-1;
            else
                right = mid-1;   //向左侧边界收缩
        }
        //特判 检查 left 越界的情况
        if(left>=nums.size()||nums[left]!=target)
            return -1;
        return left;
    }
```

#### 寻找右侧边界的二分搜索：

```c++
int search_right_bound(vector<int>& nums, int target) {
        if(nums.size()==0)
            return -1;
        int left = 0;
        int right = nums.size()-1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]<target)
                left = mid+1;
            else if(nums[mid]>target)
                right = mid-1;
            else
                left = mid+1;   //向右侧边界收缩
        }
        //特判 检查 right 越界的情况
        if(right<0||nums[right]!=target)
            return -1;
        return right;
    }
```

#### **相关题目：**

[704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

[35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

[74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

[278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

[153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

[154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

[33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

[81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)