---
title: Leetcode-130.Surrounded Regions
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Union Find
---

## 一、题意

### 0、题目链接
[130.Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

### 1、Description
【输入】
给定m * n的二维数组board，代表一张地图；
元素取值为'X'、'O'，分别代表：陆地、水；
【输出】
在地图board中，将所有的湖，变成陆地；
湖：水的联通块（上、下、左、右四个方向），且被陆地围起来。同时把board之外的看作是水。
【要求】
无；

### 2、Example
Input:
[
X X X X
X O O X
X X O X
X O X X
]
Output:
[
X X X X
X X X X
X X X X
X O X X
]

<!-- more -->

### 3、Corner Case
1. m == 0 || n == 0的时候，返回即可；
2. 考虑m、n均大于0的情况；

## 二、题解

### 0、思考
1. 类似于200.Number of Islands，可以使用并查集来解决。

2. 不同点：
200中，地图外是水，我们需要找出陆地的联通块的个数。
本题中，地图外仍是水，我们需要找出水的联通块，且需要被陆地包围起来，才能构成湖。

### 1、Solution-1：Union Find
1. 创建一个大小为m * n + 1的并查集对象uf，编号[0, m * n]，其中最后一个编号代表地图外的水域。

2. i从0开始，到m - 1结束。j从0开始，到n - 1结束。若board[i][j] == 'O'，则考虑(i, j)的邻居。
此处不必考虑上、下、左、右四个邻居，只要考虑两个即可。因为：遍历到“邻居”时，邻居也会考虑我。

3. 处理完所有的联通关系后，再次遍历board。若board[i][j] == 'O'，则考虑它和地图外水域的联通性。
若联通，则说明不能构成湖；否则，构成湖，将其替换为陆地；

4. Code(https://leetcode.com/submissions/detail/338071258/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if (board.size() == 0 || board[0].size() == 0) {
            return;
        }
        int m = board.size();
        int n = board[0].size();
        UnionFind uf(m * n + 1);
        int dummyNode = m * n;
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (board[i][j] == 'X') {
                    continue;
                }
                int idx = i * n + j;
                if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
                    uf.Union(idx, dummyNode);
                }
                if (i + 1 <= m - 1 && board[i + 1][j] == 'O') {
                    uf.Union(idx, (i + 1) * n + j);
                }
                if (j + 1 <= n - 1 && board[i][j + 1] == 'O') {
                    uf.Union(idx, i * n + j + 1);
                }
            }
        }
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (board[i][j] == 'X') {
                    continue;
                }
                int idx = i * n + j;
                if (uf.Find(idx) != uf.Find(dummyNode)) {
                    board[i][j] = 'X';
                }
            }
        }
    }
};
```