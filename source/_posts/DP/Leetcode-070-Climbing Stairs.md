---
title: Leetcode-070.Climbing Stairs
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[070.Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

### 1、Description
【输入】
给定一个整数n；
代表着一个有n个台阶的楼梯；
【输出】
小王每次能上1阶或者2阶；
求小王一共有几种不同的上楼梯方案，输出这个方案数；
【要求】
无；

### 2、Example
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
Way-1. 1 step + 1 step + 1 step
Way-2. 1 step + 2 steps
Way-3. 2 steps + 1 step

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 45；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：上完前i阶，一共有几种不同的方案数；

2. 答案：dp[n]；

3. 递推式：dp[i] = dp[i - 1] + dp[i - 2]；

4. Code(https://leetcode.com/submissions/detail/427448577/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) {
            return n;
        }
        
        vector<int> dp(n + 1, -1);
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
```

5. 空间优化
计算dp[i]只需要dp[i - 1]和dp[i - 2]；

6. Code(https://leetcode.com/submissions/detail/427450560/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) {
            return n;
        }
        
        int prepre = 1;
        int pre = 2;
        for (int i = 3; i <= n; i++) {
            int cur = prepre + pre;
            prepre = pre;
            pre = cur;
        }
        return pre;
    }
};
```