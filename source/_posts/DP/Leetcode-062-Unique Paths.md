---
title: Leetcode-062.Unique Paths
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[062.Unique Paths](https://leetcode.com/problems/unique-paths/)

### 1、Description
【输入】
给定一个两个整数m、n，代表着一张m * n的地图；
【输出】
小王从左上角出发，到右下角去；每次只能向右或者向下走一步；
求小王一共有多少种不同的方案到达目的地；
【要求】
无；

### 2、Example
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
Way1. Right -> Down -> Down
Way2. Down -> Down -> Right
Way3. Down -> Right -> Down

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 100；
2. ans <= 2 * 10^9；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：到达坐标(i, j)，一共有多少种不同的方案；

2. 答案：dp[m - 1][n - 1]；

3. 递推式：dp[i][j] = dp[i][j - 1] + dp[i - 1][j]；

4. 初始化
dp[0][0]-dp[0][n - 1]均为1；
dp[1][0]-dp[m - 1][0]均为1；

5. Code(https://leetcode.com/submissions/detail/427814916/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 1));
        for (int i = 1; i <= m - 1; i++) {
            for (int j = 1; j <= n - 1; j++) {
                dp[i][j] = dp[i][j - 1] + dp[i - 1][j];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

6. 空间优化
计算dp[i][j]只需要：左边元素dp[i][j - 1]和正上方元素dp[i - 1][j]；

7. Code(https://leetcode.com/submissions/detail/427815500/)
AC
时间复杂度：O(m * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<int> dp(n, 1);
        for (int i = 1; i <= m - 1; i++) {
            for (int j = 1; j <= n - 1; j++) {
                dp[j] = dp[j - 1] + dp[j];
            }
        }
        return dp[n - 1];
    }
};
```