---
title: Leetcode-547.Friend Circles
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Union Find
---

## 一、题意

### 0、题目链接
[547.Friend Circles](https://leetcode.com/problems/friend-circles/)

### 1、Description
【背景】
班级里有N个学生，他们之间存在朋友关系。
朋友关系具有传递性：若A是B的直接朋友，B是C的直接朋友，则A是C的间接朋友。从而ABC组成了一个朋友圈。
【输入】
给定n * n的二维数组matrix；元素取值为0、1；
若matrix[i][j] = 1，则说明学生i和学生j是直接朋友关系；否则他们不是直接朋友关系；
【输出】
计算班级中一共有多少个“朋友圈”；
【要求】
无；

### 2、Example
Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 200；
2. 朋友圈的数目num在范围：[1, n]中；
3. matrix[i][i] = 1，对于所有i在[0, n - 1]都成立。即每个人都是自己的朋友；在matrix中，对角线（左上-右下）上都是1；
4. matrix[i][j] = matrix[j][i]，在matrix中元素是关于对角线（左上-右下）对称。

## 二、题解

### 0、思考
1. “朋友圈”问题，使用并查集来解决。

### 1、Solution-1：Union Find
1. 创建一个大小为n的并查集对象uf。

2. i从0开始，到n - 1结束。j从i + 1开始，到n - 1结束。若matrix[i][j] == 1，则把i、j进行合并。

3. 朋友圈的数目：i从0开始，到n - 1结束，统计leader[i] == i的即可。

4. Code(https://leetcode.com/submissions/detail/337695098/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class UnionFind {
private:
    int _n;
    vector<int> leader;
    vector<int> rank;
public:
    UnionFind(int n) {
        _n = n;
        leader.resize(n);
        rank.resize(n);
        for (int i = 0; i <= n - 1; i++) {
            leader[i] = i;
            rank[i] = 1;
        }
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
        } else if (rank[fx] > rank[fy]) {
            // x当家作主。
            leader[fy] = fx;
            rank[fx] += rank[fy];
            rank[fy] = 0;
        } else {
            // y当家作主。
            leader[fx] = fy;
            rank[fy] += rank[fx];
            rank[fx] = 0;
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
};
 
class Solution {
public:
    int findCircleNum(vector<vector<int>>& M) {
        int n = M.size();
        UnionFind uf(n);
        for (int i = 0; i <= n - 1; i++) {
            for (int j = i + 1; j <= n - 1; j++) {
                if (M[i][j] == 1) {
                    uf.Union(i, j);
                }
            }
        }
        return uf.Count();
    }
};
```

