---
title: Leetcode-684.Redundant Connection
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Graph
- Tree
- Union Find
---

## 一、题意

### 0、题目链接
[684.Redundant Connection](https://leetcode.com/problems/redundant-connection/)

### 1、Description
【输入】
给定n * 2的二维数组edges，代表一张无向图的n条边；
图中有n个结点，结点编号在[1, n]范围内；
且：edges[i][0] < edges[i][1]；
【输出】
删除多余的边，使得图刚好成为一棵无根树；
若满足的边有多条，删除在edges中后出现的；
【概念】
树：一张特殊的图。特定：无环、连通、恰好有n - 1条边（其中n是结点数目）。
【要求】
无；

### 2、Example
Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]

<!-- more -->

### 3、Corner Case
1. 3 <= n <= 100； 

## 二、题解

### 0、思考
1. 一棵n个节点的树，恰好有n - 1条边；

2. 根据树是连通的性质：遍历给定的n条边，若(u, v)已经连通了，则将其返回即可。否则，将u、v这两个结点连通。

### 1、Solution-1：Union Find
1. 创建一个大小为n + 1的并查集对象uf，编号[0, n]，其中编号0不会用到。

2. i从0开始，到n - 1结束。依次考察每条边(u, v)。

3. Code(https://leetcode.com/submissions/detail/338209419/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {        
        int n = edges.size();
        UnionFind uf(n + 1);
        for (int i = 0; i <= n - 1; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            if (uf.Find(u) == uf.Find(v)) {
                return edges[i];
            }
            uf.Union(u, v);
        }
        return {}; // cannot execute
    }
};
```