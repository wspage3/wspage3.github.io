---
title: Leetcode-509.Fibonacci Number
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[509.Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

### 1、Description
【输入】
给定一个整数n；
【输出】
求斐波那契数列的第n项f[n]；
已知：
f[0] = 0，f[1] = 1；
f[i] = f[i - 1] + f[i - 2]；
【要求】
无；

### 2、Example
Input: 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 30；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. 递推式：dp[i] = dp[i - 1] + dp[i - 2]；

2. 空间优化
计算dp[i]只需要dp[i - 1]和dp[i - 2]；

3. Code(https://leetcode.com/submissions/detail/427453273/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int fib(int n) {
        if (n <= 1) {
            return n;
        }
        
        int prepre = 0;
        int pre = 1;
        for (int i = 2; i <= n; i++) {
            int cur = prepre + pre;
            prepre = pre;
            pre = cur;
        }
        return pre;
    }
};
```