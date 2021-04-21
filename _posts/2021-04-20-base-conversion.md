---
layout: post
title: '进制转换'
date: 2021-04-20
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Base conversion

```
//m转n 模拟大数除法
string change(string num, int m, int n){
    string ans = "";
    for(int i=0;i<num.length();){
        int remain = 0;
        for(int j=i;j<num.length();j++){
            int temp = remain*m+num[j]-'0';
            num[j] = temp/n+'0';
            remain = temp%n;
        }
        ans+=remain+'0';
        while(num[i]=='0'){
            i++;
        }
    }
    reverse(ans.begin(),ans.end());
    return ans;
}
```

```
#include <iostream>
#include <algorithm>
#include <string>
#include <cstdio>
#include <cstring>
using namespace std;

//小数m进制转n进制
string change(int num, int m, int n){
    string ans = "";
    //防止num=0
    do{
        int temp = num%n;
        num/=n;
        ans+=temp+'0';
    }while(num);
    reverse(ans.begin(),ans.end());
    return ans;
}


//大数m进制转n进制
string change(string num, int m, int n){
    string ans = "";
    for(int i=0;i<num.length();){
        int remain = 0;
        for(int j=i;j<num.length();j++){
            int temp = remain*m+num[j]-'0';
            num[j] = temp/n+'0';
            remain = temp%n;
        }
        ans+=remain+'0';
        while(num[i]=='0'){
            i++;
        }
    }
    reverse(ans.begin(),ans.end());
    return ans;
}



int main(){
    string s;
    while(cin>>s){
        string ans = "";
        ans = change(s,10,2);
        cout<<ans<<endl;
    }
    return 0;
}
```

[十进制和二进制](http://noobdream.com/DreamJudge/Issue/page/1176/#)

[进制转换](http://noobdream.com/DreamJudge/Issue/page/1178/#)

