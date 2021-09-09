---
title: Leetcode-063.Unique Paths II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[063.Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

### 1、Description
【输入】
给定一张m * n的地图grid；
grid[i][j]为0代表：位置(i, j)处无障碍物；
grid[i][j]为1代表：位置(i, j)处有障碍物；
【输出】
小王从左上角出发，到右下角去；每次只能向右或者向下走一步、同时不可走到有障碍物的位置；
求小王一共有多少种不同的方案到达目的地；
【要求】
无；

### 2、Example
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
Way1. Right -> Right -> Down -> Down
Way2. Down -> Down -> Right -> Right

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 100；
2. grid合法：元素要么为0，要么为1；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：到达坐标(i, j)，一共有多少种不同的方案；

2. 答案：dp[m - 1][n - 1]；

3. 递推式：
dp[i][j] = 0：该位置是障碍物；
dp[i][j] = dp[i][j - 1] + dp[i - 1][j]：该位置不是障碍物，可以由左边、上边跳过来；

4. 初始化
第一行：若grid[0][y]为第一行的第一个障碍物，则：dp[0][0]-dp[0][y - 1]为1，dp[0][y]-dp[0][n - 1]为0；
第一列：若grid[x][0]为第一列的第一个障碍物，则：dp[0][0]-dp[x - 1]0]为1，dp[x][0]-dp[m - 1][0]为0；

5. Code(https://leetcode.com/submissions/detail/427826477/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int j = 0; j <= n - 1; j++) {
            if (grid[0][j] == 1) {
                break;
            }
            dp[0][j] = 1;
        }
        for (int i = 0; i <= m - 1; i++) {
            if (grid[i][0] == 1) {
                break;
            }
            dp[i][0] = 1;
        }
        for (int i = 1; i <= m - 1; i++) {
            for (int j = 1; j <= n - 1; j++) {
                if (grid[i][j] == 1) {
                    continue;
                }
                dp[i][j] = dp[i][j - 1] + dp[i - 1][j];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

6. 空间优化
计算dp[i][j]只需要：左边元素dp[i][j - 1]和正上方元素dp[i - 1][j]；

7. Code(https://leetcode.com/submissions/detail/427828845/)
AC
时间复杂度：O(m * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<int> dp(n, 0);
        for (int j = 0; j <= n - 1; j++) {
            if (grid[0][j] == 1) {
                break;
            }
            dp[j] = 1;
        }
        for (int i = 1; i <= m - 1; i++) {
            // 若位置(i, 0)是障碍物，则dp[0] = 0；
            // 若位置(i, 0)不是障碍物，则dp[0]仍然是上一轮计算的结果；
            if (grid[i][0] == 1) {
                dp[0] = 0;
            }
            for (int j = 1; j <= n - 1; j++) {
                if (grid[i][j] == 1) {
                    dp[j] = 0;
                } else {
                    dp[j] = dp[j - 1] + dp[j];
                }
            }
        }
        return dp[n - 1];
    }
};
```