---
title: Leetcode-037.Sudoku Solver
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
- Hash Table
---

## 一、题意

### 0、题目链接
[037.Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

### 1、Description
【输入】
给定一个9 * 9的矩阵，代表一个数独棋盘；
矩阵中元素为'1'-'9'的字符；或者'.'，代表此位置暂时为空；
【输出】
求解该9 * 9的数独：将board补全；
数独规则：使得每一行、每一列、每一个3*3的小方块内，1~9恰好各出现一次；
【要求】
无；

### 2、Example
Input: board = [
    ["5","3",".",".","7",".",".",".","."],
    ["6",".",".","1","9","5",".",".","."],
    [".","9","8",".",".",".",".","6","."],
    ["8",".",".",".","6",".",".",".","3"],
    ["4",".",".","8",".","3",".",".","1"],
    ["7",".",".",".","2",".",".",".","6"],
    [".","6",".",".",".",".","2","8","."],
    [".",".",".","4","1","9",".",".","5"],
    [".",".",".",".","8",".",".","7","9"]]
Output: [
    ["5","3","4","6","7","8","9","1","2"],
    ["6","7","2","1","9","5","3","4","8"],
    ["1","9","8","3","4","2","5","6","7"],
    ["8","5","9","7","6","1","4","2","3"],
    ["4","2","6","8","5","3","7","9","1"],
    ["7","1","3","9","2","4","8","5","6"],
    ["9","6","1","5","3","7","2","8","4"],
    ["2","8","7","4","1","9","6","3","5"],
    ["3","4","5","2","8","6","1","7","9"]]

<!-- more -->

### 3、Corner Case
1. board合法：为9*9的矩阵；
2. board[i][j]合法；
3. 题目保证，对于一个输入，有且仅有一个合法解；

## 二、题解

### 0、思考
1. “数独问题”是一个经典问题；

2. 使用回溯法解决；

### 1、Solution-1：Backtracking
1. 当找到合法解的时候，立即跳出；
若不跳出，则回溯回去，对board的修改就丢失了；

2. 使用三个哈希表，记录某个位置(x, y)是否可以放置数字num；

3. Code(https://leetcode.com/submissions/detail/426420559/)
AC
// 每个结点的“度数”：最大为9
// 递归树的高度为：81
时间复杂度：O(9 ^ 81)
空间复杂度：O(81) 
```C++
class Solution {
private:
    bool usedRow[9][9] = {false};
    bool usedCol[9][9] = {false};
    bool usedGrid[9][9] = {false};
    bool backtracking(int pos, vector<vector<char>>& board) {
        if (pos == 81) {
            return true;
        }
        int x = pos / 9;
        int y = pos % 9;
        if (board[x][y] != '.') {
            return backtracking(pos + 1, board);
        }
        for (int i = 1; i <= 9; i++) {
            if (usedRow[x][i - 1] || usedCol[y][i - 1] || usedGrid[x / 3 * 3 + y / 3][i - 1]) {
                continue;
            }
            board[x][y] = i + '0';
            usedRow[x][i - 1] = true;
            usedCol[y][i - 1] = true;
            usedGrid[x / 3 * 3 + y / 3][i - 1] = true;
            if (backtracking(pos + 1, board)) {
                return true;
            }
            usedRow[x][i - 1] = false;
            usedCol[y][i - 1] = false;
            usedGrid[x / 3 * 3 + y / 3][i - 1] = false;
            board[x][y] = '.';
        }
        return false;
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        // 1. init
        for (int i = 0; i <= 8; i++) {
            for (int j = 0; j <= 8; j++) {
                if (board[i][j] == '.') {
                    continue;
                }
                int idx = board[i][j] - '0' - 1;
                usedRow[i][idx] = true;
                usedCol[j][idx] = true;
                usedGrid[i / 3 * 3 + j / 3][idx] = true;
            }
        }
        // 2. solve
        backtracking(0, board);
    }
};
```