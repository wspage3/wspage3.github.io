---
title: Leetcode-1254.Number of Closed Islands
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Union Find
---

## 一、题意

### 0、题目链接
[1254.Number of Closed Islands](https://leetcode.com/problems/number-of-closed-islands/)

### 1、Description
【输入】
给定m * n的二维数组grid，代表一张地图；
元素取值为0、1，分别代表：陆地、水；
【输出】
在地图grid中，统计岛屿（需要被水围起来）的个数；
岛屿：陆地的联通块（上、下、左、右四个方向）。同时把grid之外的看作是陆地。
【要求】
无；

### 2、Example
Input: 
[
 [0,0,1,0,0],
 [0,1,0,1,0],
 [0,1,1,1,0]
]
Output: 1

<!-- more -->

### 3、Corner Case
1. m == 0 || n == 0的时候，返回0；
2. 考虑m、n均大于0的情况；

## 二、题解

### 0、思考
1. 类似于130.Surrounded Regions，可以使用并查集来解决。

2. 相同点：
130中，地图外是水，我们需要找出水的联通块，且需要被陆地包围起来，才能构成“Lake”。
本题中，地图外是陆地，我们需要找出陆地的联通块，且需要被水包围起来，才能构成“Closed Island”。

3. 不同点：
130中，找出所有的“Lake”，将其替换为陆地；
本题中，找出所有的“Closed Island”，统计其个数；

### 1、Solution-1：Union Find
1. 创建一个大小为m * n + 1的并查集对象uf，编号[0, m * n]，其中最后一个编号代表地图外的陆地区域。

2. i从0开始，到m - 1结束。j从0开始，到n - 1结束。若grid[i][j] == 0，则考虑(i, j)的邻居。
此处不必考虑上、下、左、右四个邻居，只要考虑两个即可。因为：遍历到“邻居”时，邻居也会考虑我。

3. 处理完所有的联通关系后，再次遍历grid。若grid[i][j] == 0，则考虑它和地图外陆地区域的联通性。
若联通，则说明不能构成“Closed Island”；否则，构成，统计其个数；

4. Code(https://leetcode.com/submissions/detail/338098136/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    int closedIsland(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        UnionFind uf(m * n + 1);
        int dummyNode = m * n;
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (grid[i][j] == 1) {
                    continue;
                }
                int idx = i * n + j;
                if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
                    uf.Union(idx, dummyNode);
                }
                if (i + 1 <= m - 1 && grid[i + 1][j] == 0) {
                    uf.Union(idx, (i + 1) * n + j);
                }
                if (j + 1 <= n - 1 && grid[i][j + 1] == 0) {
                    uf.Union(idx, i * n + j + 1);
                }
            }
        }
        int cnt = 0;
        int dummyNodeLeader = uf.Find(dummyNode);
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (grid[i][j] == 1) {
                    continue;
                }
                int idx = i * n + j;
                int curLeader = uf.Find(idx);
                if (curLeader != dummyNodeLeader && curLeader == idx) {
                    cnt++;
                }
            }
        }
        return cnt;
    }
};
```