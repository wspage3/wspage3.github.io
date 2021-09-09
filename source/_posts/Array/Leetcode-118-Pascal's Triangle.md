---
title: Leetcode-118.Pascal's Triangle
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
---

## 一、题意

### 0、题目链接
[118.Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)

### 1、Description
【输入】
给定一个非负整数n；
【输出】
求出n行的杨辉三角形：每行是一个一维数组，最终输出一个二维数组；
【要求】
无；

### 2、Example
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]

<!-- more -->

### 3、Corner Case
1. n == 0时，输出空；

## 二、题解

### 1、Solution-1：Straightforward
1. Code(https://leetcode.com/submissions/detail/428892979/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<vector<int>> generate(int n) {
        vector<vector<int>> ans(n);
        if (n == 0) {
            return ans;
        }
        
        for (int i = 0; i <= n - 1; i++) {
            ans[i].resize(i + 1);
            ans[i][0] = ans[i][i] = 1;
            for (int j = 1; j <= i - 1; j++) {
                ans[i][j] = ans[i - 1][j - 1] + ans[i - 1][j];
            }
        }
        return ans;
    }
};
```