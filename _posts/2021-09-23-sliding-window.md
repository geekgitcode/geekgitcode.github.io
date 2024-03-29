---
layout: post
title: '滑动窗口/双指针'
date: 2021-09-23
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Sliding Window

**注意双指针是基于一个序列还是两个序列**

**两个序列：**i, j分别在两个不同的序列上游走  [例题](https://www.acwing.com/problem/content/2818/)

**一个序列：**滑动窗口，在一个序列上游走

```cpp
int left = 0, right = 0;

while (right < s.size()) {
    // 增大窗口
    window.add(s[right]);
    right++;
    
    while (window needs shrink) {
        // 缩小窗口
        window.remove(s[left]);
        left++;
    }
}
```

```cpp
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;
    
    int left = 0, right = 0;
    int valid = 0; 
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/
        
        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}
```

[3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int l=0,r=0;
        int n = s.length();
        unordered_map<char, int> mmap;
        int ans = 0;
        while(r<n){
            char c = s[r];
            r++;
            //窗口
            mmap[c]++;
            while(mmap[c]>1){
                char d = s[l];
                l++;
                //窗口
                mmap[d]--;
            }
            ans = max(ans,r-l);
        }
        return ans;
    }
};
```

[438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

```
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        unordered_map<char, int> need;
        unordered_map<char, int> window;
        int l=0,r=0;
        int n = s.length();
        int valid=0;
        vector<int> ans;
        for(char c:p)
            need[c]++;

        while(r<n){
            char c=s[r];
            r++;
            //需要时才进窗口
            if(need.count(c)){
                window[c]++;
                if(window[c]==need[c])
                    valid++;
            }
            //收缩时机
            while(r-l==p.length()){
                if(valid==need.size())
                    ans.push_back(l);
                char d = s[l];
                if(need.count(d)){
                    //为何加if：当window中某个字符数大于need，此时虽然减1但是valid不变
                    if(need[d]==window[d])
                        valid--;
                    window[d]--;
                }
                l++;
            }
        } 
        return ans;
    }
};
```

**滑动窗口算法的思路是这样**：

1、我们在字符串 `S` 中使用双指针中的左右指针技巧，初始化 `left = right = 0`，把索引**左闭右开**区间 `[left, right)` 称为一个「窗口」。

2、我们先不断地增加 `right` 指针扩大窗口 `[left, right)`，直到窗口中的字符串符合要求（包含了 `T` 中的所有字符）。

3、此时，我们停止增加 `right`，转而不断增加 `left` 指针缩小窗口 `[left, right)`，直到窗口中的字符串不再符合要求（不包含 `T` 中的所有字符了）。同时，每次增加 `left`，我们都要更新一轮结果。

4、重复第 2 和第 3 步，直到 `right` 到达字符串 `S` 的尽头。

这个思路其实也不难，**第 2 步相当于在寻找一个「可行解」，然后第 3 步在优化这个「可行解」，最终找到最优解，**也就是最短的覆盖子串。左右指针轮流前进，窗口大小增增减减，窗口不断向右滑动，这就是「滑动窗口」这个名字的来历。



题目：

[76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

[567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

[438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

[3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

[424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)



参考资料：

[labuladong](https://labuladong.gitee.io/algo/2/22/54/)

