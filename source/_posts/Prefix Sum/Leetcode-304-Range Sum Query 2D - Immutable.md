---
title: Leetcode-304.Range Sum Query 2D - Immutable
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Prefix Sum
- Array
---

## 一、题意

### 0、题目链接
[304.Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)

### 1、Description
【输入】
给定一个m * n的整数矩阵matrix；
给定两个坐标a(row1, col1)、b(row2, col2)；
【输出】
求出以a为左上角，以b为右下角构成的子矩阵中所有元素的和；（包含a、b）
【要求】
无；

### 2、Example
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12

<!-- more -->

### 3、Corner Case
1. matrix中的元素不会变化；
2. sumRegion函数会被频繁调用；即：尽量降低sumRegion的时间复杂度；
3. 当n == 0 || m == 0时，sumRegion函数无意义，不应该被调用；
4. row1 <= row2 && col1 <= col2；即：当a位置确定后，b要么和a重合，要么在a的右边，要么在a的下边，要么在a的右下方；

## 二、题解

### 0、思考
1. 假定数据范围同303题，不会爆signed int；

2. 303题是一维前缀和的应用；本题是二维前缀和的应用；

### 1、Solution-1：Prefix Sum
1. 二维前缀和的应用；

2. 思路：prefix[i][j]记录子矩阵(0, 0) ~ (i - 1, j - 1)内所有元素的和；即：
prefix[0][0] = 0;
prefix[0][j] = 0; j从1，到m都成立；
prefix[i][0] = 0; i从1，到n都成立；
prefix[1][1] = matrix[0][0]
prefix[1][2] = matrix[0][0] + matrix[0][1]
prefix[2][1] = matrix[0][0] + matrix[1][0]
prefix[2][2] = matrix[0][0] + matrix[0][1] + matrix[1][0] + matrix[1][1]
观察可以发现：
prefix[2][2] = prefix[1][2] + prefix[2][1] - prefix[1][1] + maxtrix[1][1]
进一步，可以得出：
prefix[i][j] = prefix[i - 1][j] + prefix[i][j - 1] - prefix[i - 1][j - 1] + maxtrix[i - 1][j - 1]

3. 初始化：为prefix数组赋值即可。时间复杂度：O(n * m)；

4. 查询：画图可以看出：
ans = prefix[row2 + 1][col2 + 1] - prefix[row2 + 1][col1] - prefix[row1][col2 + 1] + prefix[row1][col1];
时间复杂度：O(1)；

5. Code(https://leetcode.com/submissions/detail/383630318/)
AC
时间复杂度：O(n * m)-初始化；O(1)-查询；
空间复杂度：O(n * m)-初始化；O(1)-查询；
```C++
class NumMatrix {
private:
    int _n;
    int _m;
    vector<vector<int>> prefix;
public:
    NumMatrix(vector<vector<int>>& matrix) {
        int n = matrix.size();
        _n = n;
        if (n == 0) {
            return;
        }
        int m = matrix[0].size();
        _m = m;
        if (m == 0) {
            return;
        }
        
        prefix = vector<vector<int>>(n + 1, vector<int>(m + 1, 0));
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                prefix[i][j] = prefix[i - 1][j] + prefix[i][j - 1] - prefix[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        if (_n == 0 || _m == 0) {
            return -1; // invalid
        }
        int ans = prefix[row2 + 1][col2 + 1] - prefix[row2 + 1][col1] - prefix[row1][col2 + 1] + prefix[row1][col1];
        return ans;
    }
};
```

