---
title: Leetcode-119.Pascal's Triangle II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
---

## 一、题意

### 0、题目链接
[119.Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)

### 1、Description
【输入】
给定一个整数n；
【输出】
求出杨辉三角形的第n行，输出一个一维数组；
【要求】
O(n)的额外空间复杂度；

### 2、Example
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
Input: n = 3
Output: [1,3,3,1]

<!-- more -->

### 3、Corner Case
1. n < 0时，非法；

## 二、题解

### 1、Solution-1：Straightforward
1. 参考动态规划的空间优化方法：
没必要存储整个二维数组；

2. Code(https://leetcode.com/submissions/detail/428918166/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> getRow(int n) {
        if (n < 0) {
            return {};
        }
        
        vector<int> pre(n + 1);
        vector<int> cur(n + 1);
        for (int i = 0; i <= n; i++) {
            cur[0] = cur[i] = 1;
            for (int j = 1; j <= i - 1; j++) {
                cur[j] = pre[j - 1] + pre[j];
            }
            swap(pre, cur);
        }
        return pre;
    }
};
```

3. Code(https://leetcode.com/submissions/detail/428926036/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<int> getRow(int n) {
        if (n < 0) {
            return {};
        }
        
        vector<int> cur(n + 1);
        for (int i = 0; i <= n; i++) {
            cur[0] = cur[i] = 1;
            for (int j = i - 1; j >= 1; j--) {
                cur[j] = cur[j - 1] + cur[j];
            }
        }
        return cur;
    }
};
```