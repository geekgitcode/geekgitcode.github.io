---
layout: post
title: '最短路径'
date: 2021-08-30
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Shortest path



```
Floyd
1、适合求多源最短路径
2、可以求最小环
3、可以有负边权，不能有负环
4、可以求有向图的传递闭包
5、时间复杂度O(n^3)

Dijkstra
求单源、无负权的最短路。时效性较好，时间复杂度为O（V*V+E）。源点可达的话，O（V*lgV+E*lgV）=>O（E*lgV）。
当是稀疏图的情况时，此时E=V*V/lgV，所以算法的时间复杂度可为O（V^2）。若是斐波那契堆作优先队列的话，算法时间复杂度，则为O（V*lgV + E）。

spfa
是Bellman-Ford的队列优化，时效性相对好，时间复杂度O（kE）。（k<<V）。
与Bellman-ford算法类似，SPFA算法采用一系列的松弛操作以得到从某一个节点出发到达图中其它所有节点的最短路径。所不同的是，SPFA算法通过维护一个队列，使得一个节点的当前最短路径被更新之后没有必要立刻去更新其他的节点，从而大大减少了重复的操作次数。
SPFA算法可以用于存在负数边权的图，这与dijkstra算法是不同的。
```



```
//spfa 一般用邻接链表
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>
#include <cstring>
#include <queue>
#define inf 0x3f3f3f3f
using namespace std;

const int maxn = 1e2+2;

struct Edge{
    int next;
    int val;
    Edge(int next,int val):next(next),val(val){}
};
vector<Edge> edge[maxn];
bool visit[maxn];
int dis[maxn];
int path[maxn];
int cnt[maxn];    //用来判断是否出现负边环
int N,M;

void init(){
    for(int i=0;i<maxn;i++){
        edge[i].clear();
    }
    memset(visit,0,sizeof(visit));
    memset(dis,0x3f,sizeof(dis));
    memset(path,0,sizeof(path));
}

void addedge(int u,int v,int w){
    Edge temp(v,w);
    edge[u].push_back(temp);
}

void spfa(int s){
    queue<int> q;
    dis[s] = 0;
    q.push(s);
    while(!q.empty()){
        int now = q.front();
        q.pop();
        visit[now] = false;
        cnt[now]++;
        for(int i=0;i<edge[now].size();i++){
            Edge e = edge[now][i];
            if(dis[e.next]>dis[now]+e.val){
                dis[e.next]=dis[now]+e.val;
                path[e.next] = now;
                if(!visit[e.next]){
                    q.push(e.next);
                    visit[e.next] = true;
                    cnt[e.next]++;
                    if(cnt[e.next]>N)    //一个点的入队次数大于节点个数，出现负环
                    	return false;
                }
            }
        }
    }
    return true;
}

int main(){
    while(cin>>N>>M){
        if(N==0&&M==0)
            break;
        init();
        for(int i=0;i<M;i++){
            int u,v,w;
            cin>>u>>v>>w;
            addedge(u,v,w);
            addedge(v,u,w);
        }
        spfa(1);
        cout<<dis[N]<<endl;
    }
    return 0;
}
```

```
//floyd 一般用邻接矩阵
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>
#include <cstring>
#include <queue>
#define inf 0x3f3f3f3f
using namespace std;

const int maxn = 1e2+2;

int graph[maxn][maxn];

void init(){
    memset(graph,0x3f,sizeof(graph));
    for(int i=1;i<=maxn;i++){
        for(int j=1;j<=maxn;j++){
            if(i==j)
                graph[i][j]=0;
        }
    }
}

void floyd(int n){
    for(int k=1;k<=n;k++){
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(graph[i][j]>graph[i][k]+graph[k][j])
                    graph[i][j]=graph[i][k]+graph[k][j];
            }
        }
    }
}

int main(){
    int N,M;
    while(cin>>N>>M){
        if(N==0&&M==0)
            break;
        init();
        for(int i=0;i<M;i++){
            int u,v,w;
            cin>>u>>v>>w;
			if(graph[u][v]>w) //重边
            	graph[u][v] = graph[v][u] = w;
        }
        floyd(N);
        cout<<graph[1][N]<<endl;
    }
    return 0;
}
```

```
//dijstra 一般用邻接链表
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>
#include <cstring>
#include <queue>
#define inf 0x3f3f3f3f
using namespace std;

const int maxn = 1e2+2;

struct Edge{
    int next;
    int val;
    Edge(int next,int val) :next(next),val(val){}
};
struct node{
    int u;
    int dis;
    node(int u,int dis):u(u),dis(dis){}
    bool operator < (const node &a) const{
        return dis>a.dis;
    }
};
vector<Edge> edge[maxn];
bool visit[maxn];
int dis[maxn];
int path[maxn];

void init(){
    for(int i=0;i<maxn;i++){
        edge[i].clear();
    }
    memset(dis,0x3f,sizeof(dis));
    memset(visit,0,sizeof(visit));
    memset(path,0,sizeof(path));
}

void addedge(int u,int v,int w){
    Edge temp(v,w);
    edge[u].push_back(temp);
}

void dij(int s){
    priority_queue<node> pq;
    dis[s] = 0;
    pq.push(node(s,0));
    while(!pq.empty()){
        node now = pq.top();
        pq.pop();
        int u = now.u;
        if(visit[u]==0){
            visit[u] = 1;
            for(int i=0;i<edge[u].size();i++){
                Edge e = edge[u][i];
                if(dis[e.next]>dis[u]+e.val){
                    dis[e.next]=dis[u]+e.val;
                    path[e.next] = u;
                    pq.push(node(e.next,dis[e.next]));
                }
            }
        }
    }
}

int main(){
    int N,M;
    while(cin>>N>>M){
        if(N==0&&M==0)
            break;
        init();
        for(int i=0;i<M;i++){
            int u,v,w;
            cin>>u>>v>>w;
            addedge(u,v,w);
            addedge(v,u,w);
        }
        dij(1);
        cout<<dis[N]<<endl;
    }
    return 0;
}
```

[最短路](http://www.noobdream.com/DreamJudge/Issue/page/1565/)

[N诺1344题解](http://www.noobdream.com/DreamJudge/Issue/code/205191/)