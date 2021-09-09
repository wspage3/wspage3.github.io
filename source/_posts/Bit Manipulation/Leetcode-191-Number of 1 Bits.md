---
title: Leetcode-191.Number of 1 Bits
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Bit Manipulation
---

## 一、题意

### 0、题目链接
[191.Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

### 1、Description
【输入】
给定一个无符号32位整数n；
【输出】
求出n的二进制表示中，1的个数；
【要求】
无；

### 2、Example
Input: 5
Output: 2

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 4294967295

## 二、题解

### 1、Solution-1：Brute Force
1. 从最低位开始，到最高位结束：依次考察这32位；

3. Code(https://leetcode.com/submissions/detail/382222113/)
AC
时间复杂度：O(32)
空间复杂度：O(1)
```C++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ans = 0;
        while (n) {
            ans += (n & 1);
            n = n >> 1;
        }
        return ans;
    }
};
```

### 2、Solution-2：Bit Manipulation
1. 位运算
* n & (n - 1)
消掉n的最后一个1；
5：0101
4：0100
5 & 4：0100
* n & -n
得到n的最后一个1；
(13)：1101
(-13)：0011
13 & -13：0001
解释：-n，计算机中补码存储，n逐位取反后加一

2. Code(https://leetcode.com/submissions/detail/382222577/)
AC
cnt：n的二进制表示中1的个数
时间复杂度：O(cnt)
空间复杂度：O(1)
```C++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ans = 0;
        while (n) {
            ans++;
            n = n & (n - 1);
        }
        return ans;
    }
};
```

3. Code(https://leetcode.com/submissions/detail/382226053/)
AC
cnt：n的二进制表示中1的个数
时间复杂度：O(cnt)
空间复杂度：O(1)
```C++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ans = 0;
        while (n) {
            ans++;
            n -= (n & -n);
        }
        return ans;
    }
};
```