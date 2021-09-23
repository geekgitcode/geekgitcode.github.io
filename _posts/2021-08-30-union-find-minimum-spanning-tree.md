---
layout: post
title: '并查集和最小生成树'
date: 2021-08-30
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Union Find and Minimum Spanning Tree

#### 并查集

并查集是一种优雅的数据结构，主要用于解决一些**元素分组**的问题。它管理一系列**不相交的集合**，并支持两种操作：

- **合并**（Union）：把两个不相交的集合合并为一个集合。
- **查询**（Find）：查询两个元素是否在同一个集合中。

并查集的重要思想在于，**用集合中的一个元素代表集合**。

要寻找集合的代表元素，只需要一层一层往上访问**父节点**（图中箭头所指的圆），直达树的**根节点**（图中橙色的圆）即可。根节点的父节点是它自己。我们可以直接把它画成一棵树：

### 初始化

```c
int fa[MAXN];
inline void init(int n)
{
    for (int i = 1; i <= n; ++i)
        fa[i] = i;
}
```

假如有编号为1, 2, 3, ..., n的n个元素，我们用一个数组fa[]来存储每个元素的父节点（因为每个元素有且只有一个父节点，所以这是可行的）。一开始，我们先将它们的父节点设为自己。

### 查询

```c
int find(int x)
{
    if(fa[x] == x)
        return x;
    else
        return find(fa[x]);
}
```

我们用递归的写法实现对代表元素的查询：一层一层访问父节点，直至根节点（根节点的标志就是父节点是本身）。要判断两个元素是否属于同一个集合，只需要看它们的根节点是否相同即可。

### 合并

```c
inline void merge(int i, int j)
{
    fa[find(i)] = find(j);
}
```

合并操作也是很简单的，先找到两个集合的代表元素，然后将前者的父节点设为后者即可。当然也可以将后者的父节点设为前者，这里暂时不重要。本文末尾会给出一个更合理的比较方法。

只要我们在查询的过程中，**把沿途的每个节点的父节点都设为根节点**即可。下一次再查询时，我们就可以省很多事。这用递归的写法很容易实现：

### 合并（路径压缩）

```c
int find(int x)
{
    if(x == fa[x])
        return x;
    else{
        fa[x] = find(fa[x]);  //父节点设为根节点
        return fa[x];         //返回父节点
    }
}
```

以上代码常常简写为一行：

```c
int find(int x)
{
    return x == fa[x] ? x : (fa[x] = find(fa[x]));
}
```

注意赋值运算符=的优先级没有三元运算符?:高，这里要加括号。

路径压缩优化后，并查集的时间复杂度已经比较低了，绝大多数不相交集合的合并查询问题都能够解决。然而，对于某些时间卡得很紧的题目，我们还可以进一步优化。

我们用一个数组rank[]记录每个根节点对应的树的深度（如果不是根节点，其rank相当于以它作为根节点的**子树**的深度）。一开始，把所有元素的rank（**秩**）设为1。合并时比较两个根节点，把rank较小者往较大者上合并。路径压缩和按秩合并如果一起使用，时间复杂度接近 $O(N)$，但是很可能会破坏rank的准确性。

### 初始化（按秩合并）

```c
inline void init(int n)
{
    for (int i = 1; i <= n; ++i)
    {
        fa[i] = i;
        rank[i] = 1;
    }
}
```

### 合并（按秩合并）

把简单的树往复杂的树上插

```c++
inline void merge(int i, int j)
{
    int x = find(i), y = find(j);    //先找到两个根节点
    if (rank[x] <= rank[y])
        fa[x] = y;
    else
        fa[y] = x;
    if (rank[x] == rank[y] && x != y)
        rank[y]++;                   //如果深度相同且根节点不同，则新的根节点的深度+1
}
```

#### C++模板 一般做题的时候我们不需要按秩合并 见LeetCode200，547

```
class DisjSet{
    private:
        vector<int> parent;
        vector<int> rank; // 秩
    public:
        DisjSet(int max_size) : parent(vector<int>(max_size)),
                                rank(vector<int>(max_size, 0)){
            for (int i = 0; i < max_size; i++){
                parent[i] = i;
            }  
        }
        int find(int x){
            return x == parent[x] ? x : (parent[x] = find(parent[x]));
        }
        void to_union(int x1, int x2){
            int f1 = find(x1);
            int f2 = find(x2);
            if (rank[f1] > rank[f2])
                parent[f2] = f1;
            else{
                parent[f1] = f2;
                if (rank[f1] == rank[f2])
                    rank[f2]++;
                }
        }
};

//无秩版本
class DisjSet{
    private:
        vector<int> parent;
    public:
        DisjSet(int max_size) : parent(vector<int>(max_size)){
            for (int i = 0; i < max_size; i++){
                parent[i] = i;
            }  
        }
        int find(int x){
            return x == parent[x] ? x : (parent[x] = find(parent[x]));
        }
        //只需要合并和查询即可
        void to_union(int x1, int x2){
            int f1 = find(x1);
            int f2 = find(x2);
            if(f1==f2)
                return;
            parent[f2]=f1;
        }
};
```

#### 最小生成树

Kruskal 算法是一种常见并且好写的最小生成树算法，由 Kruskal 发明。该算法的基本思想是从小到大加入边，是一个贪心算法。

其算法流程为：

1. 将图 $G=\{V,E\}$ 中的所有边按照长度由小到大进行排序，等长的边可以按任意顺序。
2. 初始化图 $G'$ 为 $\{V,\varnothing\}$，从前向后扫描排序后的边，如果扫描到的边 $e$ 在 $G'$ 中连接了两个相异的连通块,则将它插入 $G'$ 中。
3. 最后得到的图 $G'$ 就是图 $G$ 的最小生成树。

在实际代码中，我们首先将这张完全图中的边全部提取到边集数组中，然后对所有边进行排序，从小到大进行枚举，每次贪心选边加入答案。使用并查集维护连通性，若当前边两端不连通即可选择这条边。

（模板使用kruskal+并查集）

```
//并查集
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>

using namespace std;

const int MAXN = 1e3+3;
int father[MAXN];

int find(int x){
    return x==father[x]?x:(father[x]=find(father[x]));
}

void init(int N){
    for(int i=1;i<=N;i++){
        father[i] = i;
    }
}

int main(){
    int N,M;
    while(cin>>N>>M){
        if(N==0)
            break;
        init(N);
        int a,b;
        int cnt = 0;
        for(int i=0;i<M;i++){
            cin>>a>>b;
            a = find(a);
            b = find(b);
            if(a!=b){
                cnt++;
                father[a] = b;
            } 
        }
        cout<<N-1-cnt<<endl;
    }
    return 0;
}
```



```
//最小生成树 Kruskal
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>

using namespace std;

const int MAXN = 102;

struct Edge{
    int a,b;
    int val;
    bool operator < (const Edge &A) const{
        return val<A.val;
    }
};
Edge edge[MAXN];

int father[MAXN];
int find(int x){
    return x==father[x]?x:(father[x]=find(father[x]));
}

void init(int N){
    for(int i=1;i<=N;i++){
        father[i] = i;
    }
}


int main(){
    int N,M;
    while(cin>>N>>M){
        if(N==0)
            break;
        init(N);
        for(int i=0;i<N;i++){
            cin>>edge[i].a>>edge[i].b>>edge[i].val;
        }
        sort(edge,edge+N);
        int ans = 0;
        int cnt = 0;
        for(int i=0;i<N;i++){
            int a = edge[i].a;
            int b = edge[i].b;
            a = find(a);
            b = find(b);
            if(a!=b){
                ans+=edge[i].val;
                cnt++;
                father[a] = b;
            } 
        }
        if(cnt==M-1)
            cout<<ans<<endl;
        else
            cout<<"?"<<endl;
    }
    return 0;
}
```

[N诺1312](http://www.noobdream.com/DreamJudge/Issue/page/1312/)

[CSDN](https://www.jianshu.com/p/fc17847b0a31)
