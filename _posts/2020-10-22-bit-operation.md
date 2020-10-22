---

layout: post
title: '位运算'
date: 2020-10-22
author: downeyking
cover: 'https://gitee.com/GoPrime/imagecloud/raw/master/bgcover/leetcode.jpg'
tags: LeetCode
---

> Bit Operation



#### 常见位操作：

1. 判断两个数是否异号

   `bool f = ((x ^ y) < 0)`  利用补码的符号位是否相同 为true时异号

2. 判断一个数是奇数还是偶数

   `a = (n&1)`   当a=0时为偶数 当a=1时为奇数

3. 变换一个数的符号

   整数取反加1，正好变成其对应的负数(补码表示)；负数取反加一，则变为其原码，即正数

   ```
   int reversal(int a) {
     return ~a + 1;
   }
   ```

4. 不用临时变量交换两个数

   ```
   a=a^b
   
   b=a^b
   
   a=a^b
   ```

5. 异或操作的特殊性

   任何一个数异或0都为它本身

   任何一个数异或它本身都为0

   ```
   a=a^0=0^a
   
   a^a=0
   
   a=a^b^b
   ```

6. 消除一个二进制数的最后一个1

   ```
   n=n&(n-1)
   ```

   <img src="https://gitee.com/GoPrime/imagecloud/raw/master/img/image-20201021215631211.png" alt="image-20201021215631211" style="zoom:50%;" />

7. 保留一个整数的二进制表示的最低位的1，其余全部位设置为0

   [在树状数组中具有重要作用](https://zhuanlan.zhihu.com/p/93795692)

   `n & -n`

   或者

   `n ^ (n & (n - 1))`

8. 利用&1和>>进行逐位读取

   - [ ] 快速幂

   ```
   计算3的13次方 将13化为二进制并把等式转化为如下形式
   3^1101 = 3^0001 * 3^0100 * 3^1000
   
   如题为计算某个数tmp的n次方 
   int pow(int n){
       int sum = 1;
       int tmp = 3;
       while(n != 0){
           if(n & 1 == 1){   //取最低位
               sum *= tmp;
           }
           tmp *= tmp;
           n = n >> 1;    //移位到下一位
       }
   
       return sum;
   }
   ```
   
   - [ ] 颠倒二进制位
   
     ```
     uint32_t reverseBits(uint32_t n) {
             uint32_t ans=0;
             int i=32;
             while(i--){
                 ans<<=1;
                 ans+=n&1;
                 n>>=1;
             }
             return ans;
         }
         
     //另一解法
     n = (n & 0x55555555) << 1 | (n >>> 1) & 0x55555555;
     n = (n & 0x33333333) << 2 | (n >>> 2) & 0x33333333;
     n = (n & 0x0f0f0f0f) << 4 | (n >>> 4) & 0x0f0f0f0f;
     n = (n << 24) | ((n & 0xff00) << 8) |
                 ((n >>> 8) & 0xff00) | (n >>> 24);
     return n;
     ```
   
     
   
9. 找出不大于N的最大的2的幂指数

   把二进制中**最左边的 1 保留，后面的 1 全部变为 0**。

   1. 找到最左边的 1，然后把它右边的所有 0 变成 1
   2. 把得到的数值加 1
   3. 把2中得到的数向右移动一位

   ```
   int findN(int n){
       n |= n >> 1;
       n |= n >> 2;
       n |= n >> 4;
       n |= n >> 8；
       n |= n >> 16 // 整型一般是 32 位，或到位数/2。
       return (n + 1) >> 1;
   }
   ```

10. 不用加减乘除做加法

   ```
   //step1:异或查看两个数进行加法操作后的结果
   //step2:与运算计算出想对应的位置的进位结果，然后左移一位
   //b代表的是两数相加是否有进位，有的话就继续，没有的话就结束得出相加后的答案
   public class Solution{
       int Add(int a,int b){
           while(b != 0){
               int temp = a ^ b;//计算出相对应的位置相加后的结果
               b = (a & b) << 1;//计算出想对应的位置的进位，然后左移一位
               a = temp;
           }
           return a;
       }
   }
   
   //这个其实可以简写成return (a^b)+((a&b)<<1);
   ```

11. 两数的中位数

    ```
    void Test(int a,int b){
        //通过位运算不会造成溢出
    	int mid = a + (a - b) >> 1;
    }
    ```

12. 两数的平均数

    ```
    int average(int a,int b){
    	return (a & b)+((a ^ b) >> 1);
    }
    ```

13. 判断一个整数n的二进制表示中第i位是否为1

    `n & (1 << i)`

14. 将一个整数n的二进制表示的第i位设置为1

    `n | (1 << i)`

#### **相关题目：**

[50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

[191. 位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)

[338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

[190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

[201. 数字范围按位与](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/)

[137. 只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)

[260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

[231. 2的幂](https://leetcode-cn.com/problems/power-of-two/)

[461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

[477. 汉明距离总和](https://leetcode-cn.com/problems/total-hamming-distance/)



#### 参考资料

[九章算法](https://mp.weixin.qq.com/s?__biz=MzU2OTUyNzk1NQ==&mid=2247490892&amp;idx=1&amp;sn=2021f5f22d96f08d9d62931b634a4178&source=41#wechat_redirect)

[Hacker's Delight笔记](https://www.cnblogs.com/Five100Miles/p/8458380.html)

[Bit Twiddling Hacks](http://graphics.stanford.edu/~seander/bithacks.html#ReverseParallel)

[labuladong](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/chang-yong-de-wei-cao-zuo)

[知乎](https://zhuanlan.zhihu.com/p/148790042)



