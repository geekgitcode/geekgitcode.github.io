---
layout: post
title: '哈夫曼树'
date: 2021-08-30
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Huffman Tree



[相关定义：](https://blog.csdn.net/qq_29519041/article/details/81428934)

权值的求法：赫夫曼树的带权路径之和为各叶子节点的权值与路径长度（层次数减一或从根节点到达叶子节点路径上的非叶子节点的个数）之积的和。在构造赫夫曼树时，非叶子节点也会带有权值（为其子节点的权值之和），当加上一个非叶子节点的权值时相当于加上以其为根节点的子树的所有叶子节点的权值。而加上从根节点到叶子节点路径上的非叶子节点权值，就包含了该叶子节点的带权路径长度。所以带权路径之和即为所有非叶子节点的权值之和。

```
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>
#include<cstring>
#include<queue>
#include<unordered_map>
#include<cstdio>
using namespace std;

const int maxn = 1e3+3;

unordered_map<char,int> umap;

void init(){
    umap.clear();
}

int huffman(int n){
    int wpl = 0;
    //最小堆
    priority_queue<int,vector<int>,greater<int> > pq;
    for(auto n:umap)
        pq.push(n.second);
    if(pq.size()==1)
        wpl = pq.top();
    while(pq.size()>1){
        int w1 = pq.top();
        pq.pop();
        int w2 = pq.top();
        pq.pop();
        wpl+=(w1+w2);
        pq.push(w1+w2);
    }
    return wpl;
}

int main(){
    string s;
    while(cin>>s){
        if(s=="END")
            break;
        init();
        int n = s.length();
        for(int i=0;i<n;i++){
            umap[s[i]]++;
        }
        int ans = 0;
        ans = huffman(n);
        double ratio = (double)n*8/ans;
        printf("%d %d %.1f\n",n*8,ans,ratio);
    }
    return 0;
}
```

[N诺1562](http://www.noobdream.com/DreamJudge/Issue/page/1562/)

