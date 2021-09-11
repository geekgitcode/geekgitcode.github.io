---
layout: post
title: '树状数组'
date: 2020-10-28
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode

---

> Binary Index Tree



树状数组（Binary Index Tree, BIT支持两种操作，时间复杂度均为 $O\left(logN\right)$

- **单点修改**：更改数组中一个元素的值
- **区间查询**：查询一个区间内所有元素的和

需要的空间为 $O\left(N\right)$
初始化树状数组的时间为 $O\left(NlogN\right)$
一般用来求区间和或者逆序对问题


步骤：

1. 对于给定长度为 $n$ 的普通数组，新建一个长度为 $n+1$ 的BIT数组。初始化 $BIT[0]=0$ 作为$dummy$节点。

2. 计算孩子节点的 $index$ 创建树。 (通过 $n\&\left(n-1\right)$来进行 $index$ 的计算)。即 $parent$ 节点加 $n\&\left(n-1\right)$ 为子节点的 $index$ 。同理子节点减  $n\&\left(n-1\right)$ 可以得到 $parent$ 节点的 $index$ 。

   <img src="https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20201028194001960.png" style="zoom:50%;" />

3. 进行 $sum$ 查询，则通过不断累加 $current\ sum$ 和父节点的值，自底向上。

   ```c++
   int getSum(int BITree[], int index) { 
       int sum = 0; // Iniialize result 
     
       // index in BITree[] is 1 more than the index in arr[] 
       index = index + 1; 
     
       // Traverse ancestors of BITree[index] 
       while (index>0) { 
           // Add current element of BITree to sum 
           sum += BITree[index]; 
     
           // Move index to parent node in getSum View 
           index -= index & (-index); 
       } 
       return sum; 
   } 
   ```

4. 更新操作，更新了一个节点后相关的子节点的值都要变化。

   ```c++
   // Updates a node in Binary Index Tree (BITree) at given index 
   // in BITree. The given value 'val' is added to BITree[i] and  
   // all of its ancestors in tree.
   void updateBIT(int BITree[], int index, int val) { 
       // index in BITree[] is 1 more than the index in arr[] 
       index = index + 1; 
     
       // Traverse all ancestors and add 'val' 
       while (index < BITree.size()) {     // 
       // Add 'val' to current node of BI Tree 
       BITree[index] += val; 
     
       // Update index to that of parent in update View 
       index += index & (-index); 
       } 
   } 
   ```

   <img src="https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20201028193112676.png" alt="image-20201028193112676" style="zoom:50%;" />

   

5. 创建BIT数组，即从第一个节点开始利用原数组的值进行 $update$ 即可。

   ```c++
   int* createTree(int arr[]){
   	int BITree[] = new int[arr.size()+1];
   	for(int i=1; i <= arr.size; i++){
   		updateBIT(BITree, i, arr[i-1]);
   	}
   	return BITree;
   }
   
   int main(){
   	int arr[6] = {1,2,3,4,5,6};
   	int *binaryIndexedTree = createTree(arr);
       //一些操作
       return 0;
   }
   ```



### 另外一种写法：

#### lowbit函数

```
inline int lowbit(int n){
   	return n&(-n);
}
```

#### 单点修改

```cpp
int tree[MAXN];
inline void update(int i, int x){
    for (int pos = i; pos < MAXN; pos += lowbit(pos))
        tree[pos] += x;
}
```

#### 求前n项和

```cpp
inline int query(int n){
    int ans = 0;
    for (int pos = n; pos; pos -= lowbit(pos))
        ans += tree[pos];
    return ans;
}
```

#### 区间查询

```cpp
inline int query(int a, int b){
    return query(b) - query(a - 1);
}
```

初始化的时候，我们只需要update每个点的初始值即可。
如直接在main函数中：

```c++
for (int i = 1; i <= n; ++i){
	scanf("%d", &x);
	update(i, x);
}
```

封装成类

```
class BIT {
private:
    vector<int> tree;
    int n;

public:
    BIT(int _n): n(_n), tree(_n + 1) {}

    static constexpr int lowbit(int x) {
        return x & (-x);
    }

    void update(int x, int d) {
        while (x <= n) {
            tree[x] += d;
            x += lowbit(x);
        }
    }

    int query(int x) const {
        int ans = 0;
        while (x) {
            ans += tree[x];
            x -= lowbit(x);
        }
        return ans;
    }
};
```

新模板 树状数组是从1开始 注意查询的时候索引相对应+1

```
class NumArray {
public:
    vector<int> tree;

    int lowbit(int x) {
        return x & -x;
    }
    // 查询前缀和
    int query(int x) {
        int ans = 0;
        for(int i = x; i > 0; i -= lowbit(i))
            ans += tree[i];
        return ans;
    }
	// 在树状数组 x 位置中增加值 u
    void add(int x, int u) {
        for(int i = x; i <= n; i += lowbit(i))
            tree[i] += u;
    }

    vector<int> nums;
    int n;

    NumArray(vector<int>& nums) {
        this->nums = nums;
        n = nums.size();
        tree.resize(n+1, 0);
        // 初始化「树状数组」，树状数组是从 1 开始
        for(int i = 0; i < n; i++)
            add(i+1, nums[i]);
    }
    
    //建完树后更新数组 原有的值是 nums[i]，要使得修改为 val，需要增加 val - nums[i]
    void update(int index, int val) {
        add(index+1, val-nums[index]);
        nums[index] = val;
    }
    
    //区间查询 
    int sumRange(int left, int right) {
        return query(right+1) - query(left);
    }

};
```

#### **相关题目：**

[洛谷 P1428](https://www.luogu.com.cn/problem/P1428)

[HDOJ P1166](http://acm.hdu.edu.cn/showproblem.php?pid=1166)

[洛谷 P1908](https://www.luogu.com.cn/problem/P1908)

[LeetCode](https://leetcode-cn.com/tag/binary-indexed-tree/)

[315. 计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)

#### 逆序对模板 （对于maxn较大的开不了数组要进行离散化）

```c++
//输出逆序对个数时比较容易 直接归并排序模板在交换的时候ans+=m-i+1
//输出位置时要绑定index并且在<=的情况下再ans+=j-m-1 见leetcode 315

#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
#define ll long long
#define maxn 100005

using namespace std;

int c[maxn], n;
int low_bit(int i){
	return i & (-i);
}

void update(int i, int v){
	while (i <= n) {
		c[i] += v;
		i += low_bit(i);
	}
}

int get_sum(int i){
	int res = 0;
	while (i) {
		res += c[i];
		i -= low_bit(i);
	}
	return res;
}

int main(){
	while (scanf("%d", &n), n){
		memset(c, 0, sizeof(c));
		int ans = 0;
		for (int i = 1; i <= n; i++){
			int a;
			scanf("%d", &a);
			update(a, 1);
		}
		printf("%d\n", ans);
	}
	return 0;
}

//离散化
class Solution {
public:
    vector<int> tree;
    int n;

    int lowbit(int x){
        return x&(-x);
    }

    void add(int x, int v){
        for(int i=x;i<=n;i+=lowbit(i))
            tree[i] += v;
    }

    int query(int x){
        int ans = 0;
        for(int i=x;i>0;i-=lowbit(i))
            ans += tree[i];
        return ans;
    }

    vector<int> countSmaller(vector<int>& nums) {
        //离散化
        n = nums.size();
        vector<int> mapping(nums.begin(),nums.end());
        sort(nums.begin(),nums.end());
        nums.erase(unique(nums.begin(),nums.end()),nums.end());
        for(int i=0;i<n;i++)
            mapping[i] = lower_bound(nums.begin(),nums.end(),mapping[i])-nums.begin()+1;

        tree.resize(n+1);

        vector<int> ans(n);
        for(int i=n-1;i>=0;i--){
            //当前位置前面的桶就是小于当前位置的数
            ans[i] = query(mapping[i]-1);
            add(mapping[i],1);
        }

        return ans;
    }
};
```

#### 参考资料

[知乎](https://zhuanlan.zhihu.com/p/93795692)

[geeksforgeeks](https://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/)

[youtube](https://www.youtube.com/watch?v=CWDQJGaN1gY)

[github](https://github.com/mission-peace/interview/blob/master/src/com/interview/tree/FenwickTree.java)

[bit做逆序对问题题解](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/solution/chi-san-hua-shu-zhuang-shu-zu-pythonshi-xian-by-mi/)

[bit一些应用](https://zhuanlan.zhihu.com/p/344360991)

