---
title: Leetcode-174.Dungeon Game
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[174.Dungeon Game](https://leetcode.com/problems/dungeon-game/)

### 1、Description
【输入】
给定一个m * n的矩阵grid，代表一张地图；
公主位于右下角；
骑士将要从左上角出发；
grid[i][j] = 0，代表：位置(i, j)处是一块空地；
grid[i][j] > 0，代表：位置(i, j)处加血量；
grid[i][j] < 0，代表：位置(i, j)处扣血量；
【输出】
骑士要去找公主；每次只能向右或者向下走一步；
当骑士血量为0或者负数时，立马去世；
求骑士出发时，至少拥有多少血量，才能顺利找到公主；
【要求】
无；

### 2、Example
Input: grid = 
[
    [-2,-3,3],
    [-5,-10,1],
    [10,30,-5]
]
Output: 7

<!-- more -->

### 3、Corner Case
1. 骑士血量无上限；

## 二、题解

### 0、思考
1. 路径方向
Leetcode-062-Unique Paths
Leetcode-063-Unique Paths II
上述两题中，我们求：从左上角到右下角一共有几种不同的路径；
不关心路径顺序：左上到右下、与右下到左上，答案都是相同的；

2. 而本题中：左上到右下、与右下到左上，答案都是不同的；
例如：grid = [[-5, 10]]
左上出发，答案为：6
右下出发，答案为：1

3. 计算顺序：从目的地，向前推算；

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：从(i, j)出发，到达坐标(m - 1, n - 1)，最少初始化拥有多少血量；

2. 答案：dp[0][0]；

3. 递推式
根据当前位置的类型，有以下三种情况：
令x = dp[i][j + 1]，y = dp[i + 1][j]；
* 空地：dp[i][j] = min(x, y);
例如：x = 3，y = 5：则到达(i, j)时拥有3点血，即可顺利找到公主；

* 加血：dp[i][j] = max(1, min(x, y) - grid[i][j]);
例如：x = 3，y = 5，grid[i][j] = 10：则到达(i, j)时拥有1点血，即可顺利找到公主；
例如：x = 3，y = 5，grid[i][j] = 1：则到达(i, j)时拥有2点血，即可顺利找到公主；

* 掉血：dp[i][j] = -grid[i][j] + min(x, y);
例如：x = 3，y = 5，grid[i][j] = -10：则到达(i, j)时拥有13点血，即可顺利找到公主；

4. Code(https://leetcode.com/submissions/detail/432962524/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& grid) {
        if (grid.size() == 0 || grid[0].size() == 0) {
            return -1; // invalid
        }
        
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n));
        // 1. init
        dp[m - 1][n - 1] = grid[m - 1][n - 1] >= 0 ? 1 : -grid[m - 1][n - 1] + 1;
        for (int i = m - 2; i >= 0; i--) {
            if (grid[i][n - 1] < 0) {
                dp[i][n - 1] = -grid[i][n - 1] + dp[i + 1][n - 1];
            } else {
                dp[i][n - 1] = max(1, dp[i + 1][n - 1] - grid[i][n - 1]);
            }
        }
        for (int j = n - 2; j >= 0; j--) {
            if (grid[m - 1][j] < 0) {
                dp[m - 1][j] = -grid[m - 1][j] + dp[m - 1][j + 1];
            } else {
                dp[m - 1][j] = max(1, dp[m - 1][j + 1] - grid[m - 1][j]);
            }
        }
        // 2. solve
        for (int i = m - 2; i >= 0; i--) {
            for (int j = n - 2; j >= 0; j--) {
                if (grid[i][j] < 0) {
                    dp[i][j] = -grid[i][j] + min(dp[i][j + 1], dp[i + 1][j]);
                } else {
                    dp[i][j] = max(1, min(dp[i][j + 1], dp[i + 1][j]) - grid[i][j]);
                }
            }
        }        
            
        return dp[0][0];
    }
};
```

5. 空间优化
将O(m * n)的空间复杂度优化为O(n)；