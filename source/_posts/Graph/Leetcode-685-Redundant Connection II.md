---
title: Leetcode-685.Redundant Connection II
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
[685.Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii/)

### 1、Description
【输入】
给定n * 2的二维数组edges，代表一张有向图的n条边；
图中有n个结点，结点编号在[1, n]范围内；
(u, v)代表：由u指向v的一条有向边；
【输出】
删除多余的边，使得图刚好成为一棵有根树；
若满足的边有多条，删除在edges中后出现的边；
【概念】
树：一张特殊的图。特定：无环、连通、恰好有n - 1条边（其中n是结点数目）。
【要求】
无；

### 2、Example
Input: [[1,2], [2,3], [3,4], [4,1], [1,5]]
Output: [4,1]

<!-- more -->

### 3、Corner Case
1. 3 <= n <= 100； 

## 二、题解

### 0、思考
1. 一棵n个节点的有根树：恰好有n - 1条边；root的入度为0，其它结点的入度均为1；

2. 由于存在一条多余的边，可以分为以下情况：
多余的边指向root结点：图中n个结点的入度均为1；
多余的边指向非root结点，图中某个结点的入度为2；则说明指向它的两条边中，一条是多余的；

### 1、Solution-1：Union Find
1. 分为两种情况进行处理：
情况1. 所有结点入度为1，将后出现的冗余边删除即可；
情况2. 某个结点node入度为2。尝试删除后出现的边ans2，然后判断是否有环路，决定答案是ans2的还是ans1。

2. Code(https://leetcode.com/submissions/detail/338299036/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        
        vector<int> ans1;
        vector<int> ans2;
        
        // step-1：统计每个结点的入度，记录其父结点；
        vector<int> father(n + 1, 0); // idx为0没使用到；
        for (int i = 0; i <= n - 1; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            if (father[v] != 0) {
                ans1 = {father[v], v};
                ans2 = {u, v};
                // 将这条后出现的边标记删除
                edges[i][0] = -1;
                edges[i][1] = -1;
                break;
            } else {
                father[v] = u;
            }
        }
        
        // step-2：通过并查集，处理结点之间的联通关系；
        UnionFind uf(n + 1); // 编号为0没使用到；
        for (int i = 0; i <= n - 1; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            // 若遇到了我们标记删除的边，跳过；
            if (u == -1 && v == -1) {
                continue;
            }
            // 若仍有环路，有两种情况：
            // 情况1. 所有结点入度为1，将该边删除即可；
            // 情况2. 某个结点node入度为2，且指向node的第二条边已标记删除。说明第一条边是答案；
            if (uf.Find(u) == uf.Find(v)) {
                return ans1.size() == 0 ? edges[i] : ans1;
            }
            uf.Union(u, v);
        }
        // 执行到此处，一定是情况2。且指向node的第二条边是答案。因为将其删除后，就无环了。
        return ans2;
    }
};
```