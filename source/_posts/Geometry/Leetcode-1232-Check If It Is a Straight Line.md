---
title: Leetcode-1232.Check If It Is a Straight Line
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Geometry
- Math
---

## 一、题意

### 0、题目链接
[1232.Check If It Is a Straight Line](https://leetcode.com/problems/check-if-it-is-a-straight-line/)

### 1、Description
【输入】
给定一个大小为n的二维数组coordinates；
coordinates[i] = [x, y]，代表着二维空间内的一个点；
【输出】
判断给定的n个点，按照其先后顺序，能否构成一条直线；
【要求】
无；

### 2、Example
Input: coordinates = [[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]
Output: true

<!-- more -->

### 3、Corner Case
1. 2 <= n <= 1000
2. coordinates内无重复的点
3. -10^4 <= coordinates[i][0], coordinates[i][1] <= 10^4

## 二、题解

### 1、Solution-1：Math
1. 求出前两个点的斜率；

2. 遍历剩下的n - 2个点，i从2开始，到n - 1结束，求出它和第一个点的斜率。判断斜率是否相同。

3. 由于可能除以0，所以转化成乘法运算；

4. Code(https://leetcode.com/submissions/detail/342903923/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool checkStraightLine(vector<vector<int>>& coordinates) {
        int n = coordinates.size();
        if (n == 2) {
            return true;
        }
        
        // (y - y0) / (x - x0) = dy / dx
        // 由于除数可能为0，所以将其转化为：
        // dx * (y - y0) = dy * (x - x0)
        int dy = coordinates[1][1] - coordinates[0][1];
        int dx = coordinates[1][0] - coordinates[0][0];
        for (int i = 2; i <= n - 1; i++) {
            if (dx * (coordinates[i][1] - coordinates[0][1]) != dy * (coordinates[i][0] - coordinates[0][0])) {
                return false;
            }
        }
        return true;
    }
};
```

