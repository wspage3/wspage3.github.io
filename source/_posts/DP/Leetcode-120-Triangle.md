---
title: Leetcode-120.Triangle
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[120.Triangle](https://leetcode.com/problems/triangle/)

### 1、Description
【输入】
给定一个长度为n的二维数组triangle；
其中第1行有1个元素，第n行有n个元素；
【输出】
小王从第1行出发，到第n行去；
求出一条路径，这条路径上的元素sum加起来最小，输出这个sum；
【要求】
O(n)的空间复杂度；

### 2、Example
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

<!-- more -->

### 3、Corner Case
1. n == 0时，输出0；
2. 考虑n >= 1的情况；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：从最后一行出发，到达坐标(i, j)，路径上的最小sum；

2. 答案：dp[0][0]；

3. 递推式：dp[i][j] = triangle[i][j] + min(dp[i + 1][j], dp[i + 1][j + 1]);

4. 初始化
dp[n - 1][0] = triangle[n - 1][0];
...
dp[n - 1][n - 1] = triangle[n - 1][n - 1];

5. Code(https://leetcode.com/submissions/detail/428017788/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n * n)
```C++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        if (n == 0) {
            return 0;
        }
        
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int j = 0; j <= n - 1; j++) {
            dp[n - 1][j] = triangle[n - 1][j];
        }
        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[i][j] = triangle[i][j] + min(dp[i + 1][j], dp[i + 1][j + 1]);
            }
        }
        return dp[0][0];
    }
};
```

6. 空间优化
计算dp[i][j]只需要：正下方元素dp[i + 1][j]和右下方元素dp[i + 1][j + 1]；

7. Code(https://leetcode.com/submissions/detail/428018996/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        if (n == 0) {
            return 0;
        }
        
        vector<int> dp(n, 0);
        for (int j = 0; j <= n - 1; j++) {
            dp[j] = triangle[n - 1][j];
        }
        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[j] = triangle[i][j] + min(dp[j], dp[j + 1]);
            }
        }
        return dp[0];
    }
};
```