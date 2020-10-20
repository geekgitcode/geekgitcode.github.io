---
layout: post
title: '回溯专题'
date: 2020-09-25
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Backtrack and DFS


### 一些「回溯」算法的问题

#### 题型一：排列、组合、子集相关问题

提示：这部分练习包含「回溯算法」的一些概念和通用的解题思路。解题的步骤是：先画图，再编码。去思考可以剪枝的条件， 为什么有的时候用 visited数组，有的时候设置搜索起点 begin 变量，理解状态变量设计的想法。

[46. 全排列](https://leetcode-cn.com/problems/permutations/)

[47. 全排列 II ](https://leetcode-cn.com/problems/permutations-ii/)     思考为什么造成了重复，如何在搜索之前就判断这一支会产生重复；

[60. 第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)     利用了剪枝的思想，减去了大量枝叶，直接来到需要的叶子结点；

[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

[40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

[77. 组合](https://leetcode-cn.com/problems/combinations/)

[78. 子集](https://leetcode-cn.com/problems/subsets/)

[90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)       剪枝技巧同 47 题、39 题、40 题；

[93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

#### 题型二：Flood Fill

提示：Flood 是「洪水」的意思，Flood Fill 直译是「泛洪填充」的意思，体现了洪水能够从一点开始，迅速填满当前位置附近的地势低的区域。类似的应用还有：PS 软件中的「点一下把这一片区域的颜色都替换掉」，扫雷游戏「点一下打开一大片没有雷的区域」。

[733. 图像渲染](https://leetcode-cn.com/problems/flood-fill/)

[200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

[130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

[79. 单词搜索](https://leetcode-cn.com/problems/word-search/)     有解即可

说明：以上问题不建议修改输入数据，设置 visited 数组是标准的做法。

#### 题型三：字符串中的回溯问题

提示：字符串的问题的特殊之处在于，字符串的拼接生成新对象，因此在这一类问题上没有显示「回溯」的过程，但是如果使用 StringBuilder 拼接字符串就另当别论。

[17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

[784. 字母大小写全排列](https://leetcode-cn.com/problems/letter-case-permutation/)

[22. 括号生成 ](https://leetcode-cn.com/problems/generate-parentheses/)      这道题广度也可利用广度优先遍历，可以通过这个问题理解一下为什么回溯算法都是深度优先遍历，并且都用递归来写。

#### 题型四：游戏问题

有些教程把回溯叫做暴力搜索，但回溯没有那么暴力，回溯是有方向地搜索。

[51. N 皇后](https://leetcode-cn.com/problems/n-queens/)     其实就是全排列问题，注意设计清楚状态变量，在遍历的时候需要记住一些信息，空间换时间；

[52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)

[36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

[37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

[488. 祖玛游戏](https://leetcode-cn.com/problems/zuma-game/)

[529. 扫雷游戏](https://leetcode-cn.com/problems/minesweeper/)

#### 排列问题和组合问题的区别：

- 排列问题：由于要记录哪一些数字已经被选择过，好让以后能正确选到未选择的数，因此需要设置 $visited$ 数组；
- 组合问题：由于顺序无关紧要，因此一个数有没有被选过很重要，因此需要设置搜索起点$start$。

建议是：

- 理解代码的时候，一定要结合变量和函数名的语义去理解，因此变量名和方法名一定要设置得更符合语义，因此，全排列的代码中 $start$ 命名为 $index$ 是更好的，表示当前要确定下标是 $index$ 的元素，递归到下一轮，当然是 $index + 1$；$i$ 的起始值是 $0$ 是因为要遍历筛选出当前排列还没有选择的数；
- 是不是要加 1，很多时候要看题目条件的设定，第 39 题，元素可以重复使用，所以下一轮搜索的时候还可以使用当前数，因此搜索起点不加 1；第 40 题，一个元素只能只用一次，这个元素用完了，下一轮不能再用，所以搜索起点加 1。

#### 对于只需验证有一个解：

单词搜索和解数独都是验证有解就可以立即返回 所以用bool型的dfs要快一些 也可以用void类型的 注意两者代码的区别



#### 参考资料：

[LeetCode](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)

[WeChat](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzAxODQxMDM0Mw==&action=getalbum&album_id=1318883740306948097&scene=173#wechat_redirect)