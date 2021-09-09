---
title: Leetcode-695.Max Area of Island
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Union Find
---

## 一、题意

### 0、题目链接
[695.Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

### 1、Description
【输入】
给定m * n的二维数组grid，代表一张地图；
元素取值为0、1，分别代表：水、陆地；
【输出】
计算地图grid中，最大的岛屿的面积。
岛屿：陆地的联通块（上、下、左、右四个方向）。同时把grid之外的看作是水。
面积：包含的陆地个数。
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
Output: 4

<!-- more -->

### 3、Corner Case
1. m == 0 || n == 0的时候，返回0即可；
2. 考虑m、n均大于0的情况；
3. 若grid中全部为0，即岛屿数为0，返回0；

## 二、题解

### 0、思考
1. 类似于200.Number of Islands。

2. 唯一区别：200题目中求岛屿的数目；本题中求岛屿的最大面积。

### 1、Solution-1：Union Find
1. 创建一个大小为m * n的并查集对象uf。

2. i从0开始，到m - 1结束。j从0开始，到n - 1结束。若grid[i][j] == 1，则考虑(i, j)的邻居。
此处不必考虑上、下、左、右四个邻居，只要考虑两个即可。因为：遍历到“邻居”时，邻居也会考虑我。

3. 岛屿的最大面积：在Union的时候，用rank[fx]（或rank[fy]）来更新maxSize即可。

4. Code(https://leetcode.com/submissions/detail/337785703/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class UnionFind {
private:
    int _n;
    vector<int> leader;
    vector<int> rank;
    int maxSize;
public:
    UnionFind(int n) {
        _n = n;
        leader.resize(n);
        rank.resize(n);
        for (int i = 0; i <= n - 1; i++) {
            leader[i] = i;
            rank[i] = 1;
        }
        maxSize = 0; // 初始化为0，因为可能所有元素都是“水”。
    }
    void Union(int x, int y) {
        int fx = Find(x);
        int fy = Find(y);
        if (fx == fy) {
            return;
        }
        if (rank[fx] == rank[fy]) {
            // 既然x、y实力相同，那么就x当家作主吧。
            leader[fy] = fx;
            rank[fx] += rank[fy];
            rank[fy] = 0;
            maxSize = max(maxSize, rank[fx]);
        } else if (rank[fx] > rank[fy]) {
            // x当家作主。
            leader[fy] = fx;
            rank[fx] += rank[fy];
            rank[fy] = 0;
            maxSize = max(maxSize, rank[fx]);
        } else {
            // y当家作主。
            leader[fx] = fy;
            rank[fy] += rank[fx];
            rank[fx] = 0;
            maxSize = max(maxSize, rank[fy]);
        }
    }
    int Find(int x) {
        // 路径压缩。
        return x == leader[x] ? x : leader[x] = Find(leader[x]);
    }
    int Count() {
        int numOfCircles = 0;
        for (int i = 0; i <= _n - 1; i++) {
            if (leader[i] == i) {
                numOfCircles++;
            }
        }
        return numOfCircles;
    }
    int GetMaxSize() {
        return maxSize;
    }
};
 
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        if (grid.size() == 0 || grid[0].size() == 0) {
            return 0;
        }
        int m = grid.size();
        int n = grid[0].size();
        UnionFind uf(m * n);
        int ans = 0;
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (grid[i][j] == 0) {
                    continue;
                }
                // 当有陆地存在的时候，最大岛屿面积至少为1。
                ans = 1;
                int idx = i * n + j;
                // 判断右边是否是陆地。
                if (j + 1 <= n - 1 && grid[i][j + 1] == 1) {
                    uf.Union(idx, i * n + j + 1);
                }
                // 判断下边是否是陆地。
                if (i + 1 <= m - 1 && grid[i + 1][j] == 1) {
                    uf.Union(idx, (i + 1) * n + j);
                }
            }
        }
        ans = max(ans, uf.GetMaxSize());
        return ans;
    }
};
```

