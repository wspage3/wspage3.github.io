---
title: Leetcode-279.Perfect Squares
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[279.Perfect Squares](https://leetcode.com/problems/perfect-squares/)

### 1、Description
【输入】
给定一个正整数n；
【输出】
求最少使用多少个完全平方数，使得其sum为n；
完全平方数：1,4,9,16,25...
【要求】
无；

### 2、Example
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.

<!-- more -->

### 3、Corner Case
1. n >= 1
2. 若n为完全平方数，则输出1；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：最少使用多少个完全平方数，使得其sum为i；

2. 答案：dp[n]；

3. 初始化
dp[0] = 0；（当i是完全平方数的时候，此时dp[i] = 1 + dp[0] = 1）

4. 递推式
dp[i] = min(dp[i], 1 + dp[i - j * j]);

5. Code(https://leetcode.com/submissions/detail/429139546/)
AC
时间复杂度：O(n * sqrt(n))
空间复杂度：O(n)
```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            int minCnt = INT_MAX;
            for (int j = 1; j * j <= i; j++) {
                minCnt = min(minCnt, 1 + dp[i - j * j]);
            }
            dp[i] = minCnt;
        }
        return dp[n];
    }
};
```