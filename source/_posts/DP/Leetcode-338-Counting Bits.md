---
title: Leetcode-338.Counting Bits
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Bit Manipulation
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[338.Counting Bits](https://leetcode.com/problems/counting-bits/)

### 1、Description
【输入】
给定一个整数n；
【输出】
求[0, n]这n + 1个数字中，每个数字的二进制表示中1的个数；
输出一个一维数组；
【要求】
无；

### 2、Example
Input: 5
Output: [0,1,1,2,1,2]

<!-- more -->

### 3、Corner Case
1. n >= 0

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：数字i的二进制表示中1的个数；

2. 答案：dp；

3. 初始化
dp[0] = 0；

4. 递推式
dp[i] = 1 + dp[i & (i - 1)];

5. Code(https://leetcode.com/submissions/detail/429220217/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> dp(n + 1);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            dp[i] = 1 + dp[i & (i - 1)];
        }
        return dp;
    }
};
```