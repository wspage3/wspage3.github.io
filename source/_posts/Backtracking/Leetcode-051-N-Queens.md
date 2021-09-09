---
title: Leetcode-051.N-Queens
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
---

## 一、题意

### 0、题目链接
[051.N-Queens](https://leetcode.com/problems/n-queens/)

### 1、Description
【输入】
给定一个整数n；
【输出】
求n皇后问题中，所有的解；
每一个解用一个一维数组表示；最后返回一个二维数组；
【要求】
无；

### 2、Example
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.

<!-- more -->

### 3、Corner Case
1. n <= 0时，返回空二维数组；
2. n == 1时，有一个解，返回{ { "Q" } }；
3. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. “N皇后问题”是一个经典问题，描述如下：
目的：在一个n * n的棋盘中，放置n个棋子；
约束：取任意两个棋子x、y，它们不能出现在同一行、同一列、同一对角线上；
注：此处的对角线特指：45°和135°的对角线；

2. 暴力法：n层循环，考虑n ^ n种不同的解，分别判断是否合法；
O(n ^ n)的时间复杂度；

3. 使用回溯法解决；

### 1、Solution-1：Backtracking
1. 由于任意两个棋子，不能出现在同一行中。所以必然是：每行一个棋子；

2. i从0开始，到n - 1结束。考虑：本行的棋子应该放置在哪个位置；

3. 对于任意i，j从0开始，到n - 1结束。若(i, j)合法，则考虑下一行；否则，考虑下一个j；

4. 结束条件：当i == n时，说明0 ~ n - 1行都已经放置OK了；

5. 判断(x, y)合法：
当前考虑x行，说明[0, x - 1]行已经放置好棋子了。
首先，判断：[0, x - 1]行中的棋子是否使用了第j列；
其次，判断：当前位置左上方45°方向的对角线上，是否有棋子；
最后，判断：当前位置右上方135°方向的对角线上，是否有棋子；

6. Code(https://leetcode.com/submissions/detail/426359572/)
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
    void backtracking(int i, int n, vector<string>& board, vector<vector<string>>& ans) {
        if (i == n) {
            ans.push_back(board);
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
    vector<vector<string>> solveNQueens(int n) {
        string s(n, '.');
        vector<string> board(n, s);
        vector<vector<string>> ans;
        backtracking(0, n, board, ans);
        return ans;
    }
};
```

### 2、Solution-2：Backtracking + Bit Manipulation
1. 进一步优化：使用位运算，可以：
提升isAttack的运算速度；
降低board带来的O(n * n)的空间复杂度；