---
title: Leetcode-221.Maximal Square
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[221.Maximal Square](https://leetcode.com/problems/maximal-square/)

### 1、Description
【输入】
给定一个m * n的矩阵matrix，元素均为字符'0'或'1'；
【输出】
找出matrix中最大的正方形，均由'1'组成；输出它的面积；
【要求】
无；

### 2、Example
Input: matrix = [
    ["0","1"],
    ["1","0"]
]
Output: 1

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 300；
2. matrix合法：元素均为字符'0'或'1'；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：以坐标(i, j)为右下角的所有正方形中，满足条件的那些（均由'1'组成），其最大边长；

2. 答案
maxLen = max(dp[0][0]-dp[m - 1][n - 1])；
ans = maxLen * maxLen；

3. 递推式
* 当matrix[i][j] == '0'时：dp[i][j] = 0；
* 当matrix[i][j] != '0'时：dp[i][j] = 1 + min(dp[i][j - 1], min(dp[i - 1][j - 1], dp[i - 1][j]))；

4. 初始化
第一行，第一列中的所有元素，以其作为右下角，只能构成边长为1的正方形；
所以其dp值要么为0，要么为1；

5. Code(https://leetcode.com/submissions/detail/428473782/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        // 1. init
        int maxLen = 0;
        for (int j = 0; j <= n - 1; j++) {
            dp[0][j] = matrix[0][j] - '0';
            maxLen = max(maxLen, dp[0][j]);
        }
        for (int i = 1; i <= m - 1; i++) {
            dp[i][0] = matrix[i][0] - '0';
            maxLen = max(maxLen, dp[i][0]);
        }
        // 2. solve
        for (int i = 1; i <= m - 1; i++) {
            for (int j = 1; j <= n - 1; j++) {
                if (matrix[i][j] == '0') {
                    continue;
                }
                dp[i][j] = 1 + min(dp[i][j - 1], min(dp[i - 1][j - 1], dp[i - 1][j]));
                maxLen = max(maxLen, dp[i][j]);
            }
        }
        // 3. return
        return maxLen * maxLen;
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/428479730/)
AC
时间复杂度：O(m * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<int> pre(n, 0);
        vector<int> cur(n, 0);
        // 1. init
        int maxLen = 0;
        for (int j = 0; j <= n - 1; j++) {
            pre[j] = matrix[0][j] - '0';
            maxLen = max(maxLen, pre[j]);
        }
        // 2. solve
        for (int i = 1; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (j == 0 || matrix[i][j] == '0') {
                    cur[j] = matrix[i][j] - '0';
                } else {
                    cur[j] = 1 + min(cur[j - 1], min(pre[j - 1], pre[j]));   
                }
                maxLen = max(maxLen, cur[j]);
            }
            swap(pre, cur);
        }
        // 3. return
        return maxLen * maxLen;
    }
};
```