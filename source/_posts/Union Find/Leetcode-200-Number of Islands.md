---
title: Leetcode-200.Number of Islands
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Union Find
---

## 一、题意

### 0、题目链接
[200.Number of Islands](https://leetcode.com/problems/number-of-islands/)

### 1、Description
【输入】
给定m * n的二维数组grid，代表一张地图；
元素取值为'0'、'1'，分别代表：水、陆地；
【输出】
计算地图grid中，一共有多少个岛屿。
岛屿：陆地的联通块（上、下、左、右四个方向）。同时把grid之外的看作是水。
【要求】
无；

### 2、Example
Input:
[
11000
11000
00100
00011
]
Output: 3

<!-- more -->

### 3、Corner Case
1. m == 0 || n == 0的时候，返回0即可；
2. 考虑m、n均大于0的情况；

## 二、题解

### 0、思考
1. 类似于547.Friend Circles，“朋友圈”问题，使用并查集来解决。

2. 把陆地看作是“学生”；若其上下左右相邻，则看作是“朋友关系”。求“朋友圈”的数目。

### 1、Solution-1：Union Find
1. 创建一个大小为m * n的并查集对象uf。

2. i从0开始，到m - 1结束。j从0开始，到n - 1结束。若grid[i][j] == 1，则考虑(i, j)的邻居。
此处不必考虑上、下、左、右四个邻居，只要考虑两个即可。因为：遍历到“邻居”时，邻居也会考虑我。

3. “朋友圈”的数目：uf.Count() - numOfWater即是岛屿的数目。

4. Code(https://leetcode.com/submissions/detail/337718913/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.size() == 0 || grid[0].size() == 0) {
            return 0;
        }
        int m = grid.size();
        int n = grid[0].size();
        UnionFind uf(m * n);
        int numOfWater = 0;
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (grid[i][j] == '0') {
                    numOfWater++;
                    continue;
                }
                int idx = i * n + j;
                // 判断右边是否是陆地。
                if (j + 1 <= n - 1 && grid[i][j + 1] == '1') {
                    uf.Union(idx, i * n + j + 1);
                }
                // 判断下边是否是陆地。
                if (i + 1 <= m - 1 && grid[i + 1][j] == '1') {
                    uf.Union(idx, (i + 1) * n + j);
                }
            }
        }
        return uf.Count() - numOfWater;
    }
};
```

