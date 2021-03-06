---
layout: post
title: 'Morris遍历'
date: 2020-09-25
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Traversing binary trees simply and cheaply


### 二叉树遍历

对于一般的遍历算法，我们都是利用栈来存储之后需要再次访问的节点。最差情况下，我们需要存储整个二叉树节点。所以空间复杂度为O(n)。而Morris遍历则是将空间复杂度降到了O(1)级别。Morris遍历用到了“线索二叉树”的概念，其实就是利用了叶子节点的左右空指针来存储某种遍历前驱节点或者后继节点。因此没有使用额外的空间。

### Morris遍历的算法思想

假设当前节点为cur1，并且开始时赋值为根节点root。

假设最右侧节点cur2，并且开始时赋值为NULL

1. 判断cur1节点是否为空

2. curr1不为空

   1）如果cur1有左孩子，则先cur2 = cur1.left，再在左子树找到最右侧节点cur2，此时要保证cur2为最右节点并且cur2的右指针没有指向cur1

   - 如果cur2的右孩子为空，表示第一次到达左子树的最右端，则将右孩子指向cur1。

     即cur2.right = cur1，并更新cur1，即cur1 = cur1.left

   - 如果cur2的右孩子不为空（为cur1），表示第二次到达左子树的最右端，将其指向为空。

     cur2.right = null。（还原树结构）并更新cur1，即cur1 = cur1.right

   2）如果cur没有左孩子，cur向右更新，即（cur1 = cur1.right）

3. cur1为空时，停止遍历



相关题：

[144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

[94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

[145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

[98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)



参考链接：

[Traversing binary trees simply and cheaply](https://www.sciencedirect.com/science/article/abs/pii/0020019079900681)

[CSDN](https://blog.csdn.net/danmo_wuhen/article/details/104339630?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)