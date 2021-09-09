---
title: Leetcode-064.Minimum Path Sum
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[064.Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

### 1、Description
【输入】
给定一个m * n的矩阵grid，元素均为非负整数；
【输出】
小王从左上角出发，到右下角去；每次只能向右或者向下走一步；
求一条路径，该路径上元素的sum最小，输出这个sum；
【要求】
无；

### 2、Example
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 200；
2. 0 <= grid[i][j] <= 100；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：从(0, 0)出发，到达坐标(i, j)，路径上的最小sum；

2. 答案：dp[m - 1][n - 1]；

3. 递推式：dp[i][j] = grid[i][j] + min(dp[i][j - 1], dp[i - 1][j])；

4. 初始化
dp[0][0] = grid[0][0]
dp[0][1] = grid[0][1] + dp[0][0]
...
dp[1][0] = grid[1][0] + dp[0][0]
...

5. Code(https://leetcode.com/submissions/detail/428031357/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        dp[0][0] = grid[0][0];
        for (int j = 1; j <= n - 1; j++) {
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }
        for (int i = 1; i <= m - 1; i++) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }
        for (int i = 1; i <= m - 1; i++) {
            for (int j = 1; j <= n - 1; j++) {
                dp[i][j] = grid[i][j] + min(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

6. 空间优化
计算dp[i][j]只需要：左边元素dp[i][j - 1]和正上方元素dp[i - 1][j]；

7. Code(https://leetcode.com/submissions/detail/428033161/)
AC
时间复杂度：O(m * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<int> dp(n, 0);
        dp[0] = grid[0][0];
        for (int j = 1; j <= n - 1; j++) {
            dp[j] = dp[j - 1] + grid[0][j];
        }
        for (int i = 1; i <= m - 1; i++) {
            dp[0] = dp[0] + grid[i][0];
            for (int j = 1; j <= n - 1; j++) {
                dp[j] = grid[i][j] + min(dp[j], dp[j - 1]);
            }
        }
        return dp[n - 1];
    }
};
```