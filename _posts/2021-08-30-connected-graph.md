---
layout: post
title: '连通图'
date: 2021-08-30
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Connected Graph

连通图：并查集只有一个子点

```
#include<bits/stdc++.h>
using namespace std;
//判断是否是一棵树：连通图+indeg=0的点只有一个+其它indeg=1

const int maxn = 1e4+4;
//vector<int> edge[maxn];
int father[maxn];
//判断顶点在不在树中
bool visit[maxn];
int indeg[maxn];

void init(){
    for(int i=0;i<maxn;i++)
        father[i]=i;
    memset(visit,0,sizeof(visit));
    memset(indeg,0,sizeof(indeg));
}

int find(int x){
    return x==father[x]?x:father[x]=find(father[x]);
}

bool judge(){
    int component = 0;
    int root = 0;
    bool flag = false;
    for(int i=0;i<maxn;i++){
        if(!visit[i])
            continue;
        if(father[i]==i)
            component++;
        if(indeg[i]==0)
            root++;
        else if(indeg[i]!=1)
            return false;
    }
    if(component==1&&root==1)
        flag = true;
    if(component==0&&root==0)
        flag = true;
    return flag;
}

int main(){
    int u,v;
    init();
    int cnt = 0;
    while(cin>>u>>v){
        if(u==-1&&v==-1)
            break;
        if(u==0&&v==0){
            cnt++;
            if(judge())
                cout<<"Case "<<cnt<<" is a tree."<<endl;
            else
                cout<<"Case "<<cnt<<" is not a tree."<<endl;
            init();
        }
        else{
            indeg[v]++;
            visit[u]=1;
            visit[v]=1;
            if(find(u)!=find(v)){
                father[v] = u;
            }
        }
    }
    return 0;
}
```

