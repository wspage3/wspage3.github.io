---
title: Leetcode-059.Spiral Matrix II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Two Pointers
---

## 一、题意

### 0、题目链接
[059.Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

### 1、Description
【输入】
给定一个正整数n；
【输出】
求出n * n的螺旋矩阵；
【要求】
无；

### 2、Example
Input: n = 3
Output: 
[
    [1,2,3],
    [8,9,4],
    [7,6,5]
]

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 20

## 二、题解

### 0、思考
1. Leetcode-054-Spiral Matrix
将一个m * n的螺旋矩阵中的元素，依次读取出来；

2. 考虑如何生成一个m * n的螺旋矩阵，将数字[1, m * n]依次插入；

3. 本题要求生成一个n * n的螺旋方阵；

### 1、Solution-1：Straightforward
1. 左上角元素坐标：(leftTopX, leftTopY)
右下角元素坐标：(rightBottomX, rightBottomY)

2. 遍历顺序：
* 上面一整行，从左到右；
* 右边一列（除去第一行末尾元素、最后一行末尾元素），从上到下；
* 下面一整行，从右到左；
* 左边一列（除去第一行末尾元素、最后一行末尾元素），从下到上；

3. 考虑以左上角、右下角构成的m * n矩阵：
* m >= 3 && n >= 2：4个循环均执行
* m >= 3 && n == 1：执行循环1、2、3

* m == 2 && n >= 2：执行循环1、3
* m == 2 && n == 1：执行循环1、3

* m == 1 && n >= 1：执行循环1

4. Code(https://leetcode.com/submissions/detail/430489835/)
AC
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        int number = 1;
        int cnt = n * n;
        vector<vector<int>> ans(n, vector<int>(n));
        int leftTopX = 0;
        int leftTopY = 0;
        int rightBottomX = n - 1;
        int rightBottomY = n - 1;
        
        // 螺旋矩阵中还存在元素，需要读取出来；
        // 考虑以左上角、右下角构成的m1 * n1矩阵：见上述第3点分析；
        while (cnt) {
            // 1. 输出上面一行
            for (int j = leftTopY; j <= rightBottomY && cnt; j++) {
                ans[leftTopX][j] = number;
                number++;
                cnt--;
            }
            // 2. 输出右边一列
            for (int i = leftTopX + 1; i <= rightBottomX - 1 && cnt; i++) {
                ans[i][rightBottomY] = number;
                number++;
                cnt--;
            }
            // 3. 输出下面一行
            for (int j = rightBottomY; j >= leftTopY && cnt; j--) {
                ans[rightBottomX][j] = number;
                number++;
                cnt--;
            }
            // 4. 输出左边一列
            for (int i = rightBottomX - 1; i >= leftTopX + 1 && cnt; i--) {
                ans[i][leftTopY] = number;
                number++;
                cnt--;
            }
            leftTopX++;
            leftTopY++;
            rightBottomX--;
            rightBottomY--;
        }
        
        return ans;
    }
};
```