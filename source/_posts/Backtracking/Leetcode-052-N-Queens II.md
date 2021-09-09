---
title: Leetcode-052.N-Queens II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
---

## 一、题意

### 0、题目链接
[052.N-Queens II](https://leetcode.com/problems/n-queens-ii/)

### 1、Description
【输入】
给定一个整数n；
【输出】
求n皇后问题中，所有的解的个数；
【要求】
无；

### 2、Example
Input: 4
Output: 2

<!-- more -->

### 3、Corner Case
1. n <= 0时，返回0；
2. n == 1时，返回1；
3. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. 051中，求所有解的具体表示；

2. 本题中，求所有解的个数即可；

### 1、Solution-1：Backtracking
1. Code(https://leetcode.com/submissions/detail/426362565/)
AC
// 每个结点的“度数”：最大为n
// 递归树的高度为：n
时间复杂度：O(n!)
空间复杂度：O(n * n) // board占用n * n的空间
```C++
class Solution {
private:
    bool isAttack(int x, int y, vector<string>& board) {
        int n = board.size();
        // 1. check attack(same column)
        for (int i = 0; i <= x - 1; i++) {
            if (board[i][y] == 'Q') {
                return true;
            }
        }
        
        // 2. check attack(左上方向：45° diagonal)
        for (int i = x - 1, j = y - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return true;
            }
        }
        
        // 3. check attack(右上方向：135° diagonal)
        for (int i = x - 1, j = y + 1; i >= 0 && j <= n - 1; i--, j++) {
            if (board[i][j] == 'Q') {
                return true;
            }
        }   
        
        return false; // 不冲突了
    }
    void backtracking(int i, int n, vector<string>& board, int& ans) {
        if (i == n) {
            ans++;
            return;
        }
        // 对于指定的第i行：可以选择n个位置，j从0开始，到n - 1结束；
        for (int j = 0; j <= n - 1; j++) {
            // 若(i, j)这个位置与已有皇后冲突，则该位置不能使用；
            if (isAttack(i, j, board)) {
                continue;
            }
            board[i][j] = 'Q';
            backtracking(i + 1, n, board, ans);
            board[i][j] = '.';
        }
    }
public:
    int totalNQueens(int n) {
        string s(n, '.');
        vector<string> board(n, s);
        int ans = 0;
        backtracking(0, n, board, ans);
        return ans;
    }
};
```

### 2、Solution-2：Backtracking + Bit Manipulation
1. 进一步优化：使用位运算，可以：
提升isAttack的运算速度；
降低board带来的O(n * n)的空间复杂度；