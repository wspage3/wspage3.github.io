---
title: Leetcode-885.Spiral Matrix III
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
---

## 一、题意

### 0、题目链接
[885.Spiral Matrix III](https://leetcode.com/problems/spiral-matrix-iii/)

### 1、Description
【输入】
给定两个整数m、n，代表一个m * n的矩阵matrix；
给定一个坐标(x, y)，代表出发点；
【输出】
以出发点为中心开始，进行螺旋扩展；
输出matrix中所有元素被访问的顺序；
【要求】
无；

### 2、Example
Input: m = 3，n = 3，x = 1，y = 1
[
    [a, b, c],
    [d, e, f],
    [g, h, i]
]
Output: 
[
    [1,1],
    [1,2],
    [2,2],
    [2,1],
    [2,0],
    [1,0],
    [0,0],
    [0,1],
    [0,2]
]
Explanation: 
出发点为e；
进行顺时针螺旋扩展：e -> f -> i -> h -> g -> d -> a -> b -> c；

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 100
2. 出发点(x, y)合法；

## 二、题解

### 0、思考
1. Leetcode-054-Spiral Matrix
给定m * n的矩阵matrix，出发点为(0, 0)；
对matrix进行螺旋遍历：从外层收缩到内层；每一层的长宽不一定相等；不会访问到matrix边界外；
实现：依次考虑以左上角、右下角构成的每一层；

2. Leetcode-059-Spiral Matrix II
给定m, n，生成m * n的螺旋矩阵，使得螺旋遍历的结果为：[1, m * n]；
螺旋遍历：出发点为(0, 0)；从外层收缩到内层；每一层的长宽不一定相等；不会访问到matrix边界外；
实现：依次考虑以左上角、右下角构成的每一层；

3. 本题
给定m * n的矩阵matrix，出发点为(x, y)；
对matrix进行螺旋遍历：从内层扩展到外层；每一层的长宽相等（边长分别为：1,3,5...）；会访问到matrix边界外；
实现：从(x, y)出发，进行模拟；若在边界之外，则忽略当前元素，继续向前走；

### 1、Solution-1：Straightforward
1. 观察可以发现规律：
访问出发点；
向右走1步，并访问；
向下走1步，并访问；
向左走2步，并访问；
向上走2步，并访问；
向右走3步，并访问；
...

2. 根据上述规律，进行代码模拟；

3. Code(https://leetcode.com/submissions/detail/430852675/)
AC
时间复杂度：O(max(m, n) ^ 2)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<vector<int>> spiralMatrixIII(int m, int n, int x, int y) {
        int cnt = m * n;
        vector<vector<int>> ans;
        vector<vector<int>> dir = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}}; // 右、下、左、上
        
        ans.push_back({x, y});
        int d = 0; // 初始方向为：右；
        int len = 0; // len初始化为0，首次进入循环是向右走，会更新len为1；
        
        while (ans.size() < cnt) {
            if (d == 0 || d == 2) {
                len++;
            }
            for (int step = 1; step <= len; step++) {
                x += dir[d][0];
                y += dir[d][1];
                if (x >= 0 && x <= m - 1 && y >= 0 && y <= n - 1) {
                    ans.push_back({x, y});
                }
            }
            d = (d + 1) % 4; // 一个方向走了len步后，换下一个方向
        }
        
        return ans;
    }
};
```