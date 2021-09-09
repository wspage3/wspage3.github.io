---
title: Leetcode-054.Spiral Matrix
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Two Pointers
---

## 一、题意

### 0、题目链接
[054. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

### 1、Description
【输入】
给定一个m * n的矩阵matrix；
【输出】
对matrix进行螺旋遍历，结果输出到一个一维数组中；
【要求】
无；

### 2、Example
Input: matrix = 
[
    [1,2,3],
    [4,5,6],
    [7,8,9]
]
Output: [1,2,3,6,9,8,7,4,5]

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 10

## 二、题解

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

4. Code(https://leetcode.com/submissions/detail/430427603/)
AC
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        // 1 <= m, n <= 10
        int m = matrix.size();
        int n = matrix[0].size();
        int cnt = m * n;
        vector<int> ans;
        int leftTopX = 0;
        int leftTopY = 0;
        int rightBottomX = m - 1;
        int rightBottomY = n - 1;
        
        // 螺旋矩阵中还存在元素，需要读取出来；
        // 考虑以左上角、右下角构成的m1 * n1矩阵：见上述第3点分析；
        while (cnt) {
            // 1. 输出上面一行
            for (int j = leftTopY; j <= rightBottomY && cnt; j++) {
                ans.push_back(matrix[leftTopX][j]);
                cnt--;
            }
            // 2. 输出右边一列
            for (int i = leftTopX + 1; i <= rightBottomX - 1 && cnt; i++) {
                ans.push_back(matrix[i][rightBottomY]);
                cnt--;
            }
            // 3. 输出下面一行
            for (int j = rightBottomY; j >= leftTopY && cnt; j--) {
                ans.push_back(matrix[rightBottomX][j]);
                cnt--;
            }
            // 4. 输出左边一列
            for (int i = rightBottomX - 1; i >= leftTopX + 1 && cnt; i--) {
                ans.push_back(matrix[i][leftTopY]);
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