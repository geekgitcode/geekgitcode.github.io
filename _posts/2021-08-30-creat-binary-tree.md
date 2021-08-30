---
layout: post
title: '二叉树及构建'
date: 2021-08-30
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Binary Tree

构建二叉树的常见几种输入模板：由连续输入构建二叉树，由字符串构建二叉树，二叉搜索树

```
//由连续输入构建二叉树
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>
#include<cstring>
#include<queue>
#include<unordered_map>
#include<cstdio>
using namespace std;

struct TreeNode{
    char c;
    TreeNode *left;
    TreeNode *right;
    //TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 };
const int maxn = 1e3+3;

TreeNode* create(TreeNode* &root){
    char c;
    cin>>c;
    if(c=='0')
        return NULL;
    else{
        root = new TreeNode;
        root->c = c;
        root->left = NULL;
        root->right = NULL;
        create(root->left);
        create(root->right);
    }
    return root;
}

//将二叉树按照先序输出
void PreOrderTraverse(TreeNode* T) {  
    if (T != NULL) {  
        cout << T->c << ' ';  
        PreOrderTraverse(T->left);  
        PreOrderTraverse(T->right);  
    }  
} 

//将二叉树按照中序输出  
void InOrderTraverse(TreeNode* T) {  
    if (T != NULL) {  
        InOrderTraverse(T->left);  
        cout << T->c << ' ';  
        InOrderTraverse(T->right);  
    }  
}  

//将二叉树按照后序输出  
void PostOrderTraverse(TreeNode* T) {  
    if (T != NULL) {  
        PostOrderTraverse(T->left);  
        PostOrderTraverse(T->right);  
        cout << T->c << ' ';  
    }  
} 

//二叉树的叶子节点个数  
int Leaf(TreeNode* T) {  
    if (T == NULL) return 0;  
    if (T->left == NULL && T->right == NULL) 
        return 1;  
    return Leaf(T->left) + Leaf(T->right);  
}  

//二叉树的深度  
int Deepth(TreeNode* T) {  
    if (T == NULL) return 0;  
    int x = Deepth(T->left);  
    int y = Deepth(T->right);  
    return max(x,y) + 1;  
}  

int main(){
    TreeNode* root = NULL;
    root = create(root);
    PreOrderTraverse(root);
    cout<<endl;  
    InOrderTraverse(root); 
    cout<<endl;  
    PostOrderTraverse(root); 
    cout<<endl;  
    cout<<Leaf(root);
    return 0;
}
```



```
//由字符串构建二叉树
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>
#include<cstring>
#include<queue>
#include<unordered_map>
#include<cstdio>
using namespace std;

struct TreeNode{
    char c;
    TreeNode *left;
    TreeNode *right;
    //TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 };
const int maxn = 1e3+3;
int pos = 0;
TreeNode* create(TreeNode* &root,string s){
    if(pos==s.length())
        return NULL;
    char c = s[pos++];
    if(c=='#')
        return NULL;
    else{
        root = new TreeNode;
        root->c = c;
        root->left = NULL;
        root->right = NULL;
        create(root->left,s);
        create(root->right,s);
    }
    return root;
}

//将二叉树按照先序输出
void PreOrderTraverse(TreeNode* T) {  
    if (T != NULL) {  
        cout << T->c << ' ';  
        PreOrderTraverse(T->left);  
        PreOrderTraverse(T->right);  
    }  
} 

//将二叉树按照中序输出  
void InOrderTraverse(TreeNode* T) {  
    if (T != NULL) {  
        InOrderTraverse(T->left);  
        cout << T->c << ' ';  
        InOrderTraverse(T->right);  
    }  
}  

//将二叉树按照后序输出  
void PostOrderTraverse(TreeNode* T) {  
    if (T != NULL) {  
        PostOrderTraverse(T->left);  
        PostOrderTraverse(T->right);  
        cout << T->c << ' ';  
    }  
} 

//二叉树的叶子节点个数  
int Leaf(TreeNode* T) {  
    if (T == NULL) return 0;  
    if (T->left == NULL && T->right == NULL) 
        return 1;  
    return Leaf(T->left) + Leaf(T->right);  
}  

//二叉树的深度  
int Deepth(TreeNode* T) {  
    if (T == NULL) return 0;  
    int x = Deepth(T->left);  
    int y = Deepth(T->right);  
    return max(x,y) + 1;  
}  

int main(){
	string t;
	while(cin>>t){
		pos=0;
		TreeNode *root;
		root = create(root,t);
		InOrderTraverse(root);
		cout<<endl;
	}
	return 0;	
}
```

```
//BST 二叉搜索树
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>
#include<cstring>
#include<queue>
#include<unordered_map>
#include<cstdio>
using namespace std;

struct TreeNode{
    int c;
    TreeNode *left;
    TreeNode *right;
    //TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 };

//将二叉树按照先序输出
void PreOrderTraverse(TreeNode* T) {  
    if (T != NULL) {  
        cout << T->c << ' ';  
        PreOrderTraverse(T->left);  
        PreOrderTraverse(T->right);  
    }  
} 
 
//BST
void insert(TreeNode* &t, int key){
	if (t == NULL){
		t = new TreeNode;
        t->c = key;
        t->left = NULL;
        t->right = NULL;
		return;
	}
	if(t->c > key){
		insert(t->left, key);
	}else if(t->c < key){
		insert(t->right, key);
	}
    else{
        return;
    }
}

//建立排序二叉树
TreeNode* createBST(int data[],int n) {
	TreeNode* root=NULL;
	for(int i=0;i<n;i++)
		insert(root,data[i]);
	return root;
}

int main(){
	int n;
	int s[100];
	while(cin>>n){
        for(int i=0;i<n;i++)
            cin>>s[i];
        TreeNode* root;
        root=createBST(s,n);
        PreOrderTraverse(root);
        cout<<endl;
	}
    return 0;
}
```

```
//前序中序遍历求后序遍历 http://www.noobdream.com/DreamJudge/Issue/page/1401/#

#include <iostream>
#include <string>
#include <cstdio>
using namespace std;

struct Treenode{
    char c;
    Treenode* left;
    Treenode* right;
};


Treenode* build(string pre,string in,int prel,int prer,int inl,int inr){
    if(prel>prer||inl>inr)
        return NULL;
    Treenode* root = new Treenode;
    int index = inl;
    for(int i=inl;i<=inr;i++){
        if(pre[prel]==in[i]){
            index = i;
            break;
        }
    }
    int size = index-inl;
    root->c = pre[prel];
    root->left = build(pre,in,prel+1,prel+size,inl,index-1);
    root->right = build(pre,in,prel+size+1,prer,index+1,inr);
    return root;
}

void postorder(Treenode* root){
    if(root!=NULL){
        postorder(root->left);
        postorder(root->right);
        cout<<root->c;
    }
}

int main(){
    string pre,in;
    while(cin>>pre>>in){
        Treenode* root = new Treenode;
        root = build(pre,in,0,pre.length()-1,0,in.length()-1);
        postorder(root);
        cout<<endl;
    }
    return 0;
}
```

```
//层序遍历重建树
Treenode* create(string s){
    queue<Treenode*> q;
    int pos = 0;
    char c = s[pos++];
    Treenode* root = new Treenode;
    root->c = c;
    q.push(root);
    while(!q.empty()){
        Treenode* temp = q.front();
        q.pop();
        c = s[pos++];
        if(c=='#'){
            temp->left=NULL;
        }
        else{
            Treenode* s= new Treenode;
            s->c=c;
            s->left=NULL;
            s->right=NULL;
            temp->left=s;
            q.push(s);
        }
        c = s[pos++];
        if(c=='#'){
            temp->right=NULL;
        }
        else{
            Treenode* s= new Treenode;
            s->c=c;
            s->left=NULL;
            s->right=NULL;
            temp->right=s;
            q.push(s);
        }
    }
    return root;
}
```

```
用数组形式确保完全二叉树 下面代码解决
给一组随机的数，构成一颗完全二叉树，要求构成的树的中序遍历为这组数从小到大的排列，最终输出这棵树的层序遍历。

void postOrder(int i){
   if (b[2*i+1] != -1)
       postOrder(2*i+1);
   c[i] = a[cnt++];
   if (b[2*i+2] != -1)
       postOrder(2*i+2);
}
int main(int argc, const char * argv[]) {
   cin>>n;
   memset(b, -1, sizeof(b));
   for (int i=0; i<n; i++){
       cin>>a[i];
       b[i] = 1;
  }
   sort(a, a+n);
   postOrder(0);
   for (int i=0; i<n; i++){
       if (b[i] != -1){
           cout<<c[i]<<" ";
      }
  }
   return 0;
}
```

[N诺1109](http://www.noobdream.com/DreamJudge/Issue/page/1109/)

