---
layout: post
title: '链表专题'
date: 2020-10-20
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Linked List



#### 链表相关的核心点

- null/nil 异常处理

- dummy node 哑巴节点

  通常在头指针不确定的状况下使用

  [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

- 快慢指针

  [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

  [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

- 插入一个节点到排序链表

  [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

- 从一个链表中移除一个节点

  [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

  [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

- 翻转链表

  [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

  [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

- 合并两个链表

  [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

  [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)  归并排序

- 找到链表的中间节点（一般使用快慢指针）

  [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)



#### 用到链表核心知识点较多的题

[143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)

1. 快慢指针找中点
2. 反转中点后的链表
3. merge链表

[234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

1. 快慢指针找中点
2. 反转中点后的链表



#### 链表的一些注意事项 由82题引申的一些知识漏洞

[82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

```c++
//注意 a=b a=c 此时b不变  a=b a->next=c 此时b->next=c
```

```c++
// 测试代码验证上面的结论
class Solution {
public:
    // head = [1,2,3,4,5]
    ListNode* reverseList(ListNode* head) {
        ListNode* b = head;
        ListNode* a = b;
        ListNode* c = new ListNode(-1);
        int i=2;
        while(i--){
            a = a->next;
        }
        a->next = c;
        //return a;   // [3,-1]
        return b;   // [1,2,3,-1]
    }
};
```

```c++
// AC代码
class Solution {
public: 
    //注意 a=b a=c 此时b不变  a=b a->next=c 此时b->next=c
    ListNode* deleteDuplicates(ListNode* head) {
       ListNode* dumpy = new ListNode(0);
       dumpy->next = head; 
       ListNode* curr = head;
       ListNode* help = dumpy;
       while(curr!=NULL&&curr->next!=NULL){
           if(curr->val==curr->next->val){
                while(curr->next&&curr->val==curr->next->val){
                    curr = curr->next;
                }
                help->next = curr->next;
                curr=curr->next;
           }
           else{
               help=curr;   //相当于移动dumpy的末节点，对dumpy无影响 只是单纯移动当前位置
               curr = curr->next;
           }
       }    
       return dumpy->next;
    }
};
```
[N诺OJ 循环链表](http://noobdream.com/DreamJudge/Issue/page/1018/)

[洛谷约瑟夫环](https://www.luogu.com.cn/problem/P1996)

```
循环链表
#include <iostream>
#include <cstdio>
using namespace std;

struct node{
	int num;
	node* next;
};

node* create(int N){
    node* head = NULL;
    head = new node;
    head->num = 1;
    head->next = NULL;
    node* curr = head;
    for(int i=2;i<=N;i++){
        node* temp;
        temp = new node;
        temp->num = i;
        temp->next = NULL;
        curr->next = temp;
        curr = curr->next;
    }
    curr->next = head;
    return head;
}

int main(){
	int N;
	cin>>N;
    node* head = create(N); 
    node *p = head;
    //区分p->next!=p和p->next!=head的使用条件
    while(p->next!=p){
        node *temp = p->next;
        p = p->next->next;
        temp->next = p->next;
        node* tempdel = p;
        p = p->next;
        delete tempdel;
    }
    cout<<p->num;
    return 0;
}

//约瑟夫环问题 
#include <iostream>
#include <algorithm>
#include <string>
#include <cstdio>
#include <cstring>
using namespace std;

struct node{
	int num;
	node* next;
};

// 创建单循环链表
node* create(int N){
    node* head = NULL;
    head = new node;
    head->num = 1;
    head->next = NULL;
    node* curr = head;
    for(int i=2;i<=N;i++){
        node* temp;
        temp = new node;
        temp->num = i;
        temp->next = NULL;
        curr->next = temp;
        curr = curr->next;
    }
    
    curr->next = head;
    return head;
}

int main(){
	int n,m;
	cin>>n>>m;
    node* head = create(n); 
    node *p,*q;
    p = head;
    q = head;
    //先找尾节点
    while(q->next!=head){
        q = q->next;
    }
    int cnt = 0;int num=0;
    while(cnt!=n){
        num++;
        //不出列
        if(num%m!=0){
            p = p->next;
            q = q->next;
        }
        //出列
        else{
            //是否打印出列顺序
            //cout<<p->num<<" ";
            q->next = p->next;
            p = q->next;
            cnt++;
        }
    }
    cout<< p->num;
    return 0;
}
```

