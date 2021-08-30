---
layout: post
title: '最小生成树'
date: 2021-08-30
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Minimum Spanning Tree



一个有 n 个结点的[连通图](https://baike.baidu.com/item/连通图/6460995)的生成树是原图的极小连通子图，且包含原图中的所有 n 个结点，并且有保持图连通的最少的边。 最小生成树可以用[kruskal](https://baike.baidu.com/item/kruskal/10242089)（克鲁斯卡尔）算法或[prim](https://baike.baidu.com/item/prim/10242166)（普里姆）算法求出。

假设 WN=(V,{E}) 是一个含有 n 个顶点的连通网，则按照克鲁斯卡尔算法构造[最小生成树](https://baike.baidu.com/item/最小生成树)的过程为：先构造一个只含 n 个顶点，而边集为空的子图，若将该子图中各个顶点看成是各棵树上的根结点，则它是一个含有 n 棵树的一个森林。之后，从网的边集 E 中选取一条权值最小的边，若该条边的两个顶点分属不同的树，则将其加入子图，也就是说，将这两个顶点分别所在的两棵树合成一棵树；反之，若该条边的两个顶点已落在同一棵树上，则不可取，而应该取下一条权值最小的边再试之。依次类推，直至森林中只有一棵树，也即子图中含有 n-1条边为止。

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

