---
title: Leetcode-079.Word Search
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
---

## 一、题意

### 0、题目链接
[079.Word Search](https://leetcode.com/problems/word-search/)

### 1、Description
【输入】
给定一个m * n的矩阵board；
给定一个字符串word；
【输出】
求出board中是否存在一条路径，路径上的字符连接起来恰好是word；
连接：只能通过上下左右四个方向连接；
【要求】
无；

### 2、Example
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 200
2. board、word中仅包含大小写字母；

## 二、题解

### 1、Solution-1：Backtracking
1. 二维数组上的DFS(Backtracking)；

2. isExist(int x, int y, int pos, vector<vector<char>>& board, string& word)
含义：从坐标(x, y)出发，看是否存在一条路径，路径上的字符连接起来为：word[pos, len - 1]；
len：word的长度；

3. 到达任意一个结点时：
判断当前结点是否满足；
从当前结点的四个邻居出发，看下一个位置是否满足；

4. 空间优化：
直接在board上标记结点被访问过，而无需再开一个二维数组用于标记；

5. Code(https://leetcode.com/submissions/detail/423918386/)
AC
时间复杂度：O(m * n * 3^len) // m * n个出发点；每一次，树高len，出度3（除了起点，其它结点只能向三个方向扩展）；  
空间复杂度：O(len) // 递归树最大深度为len，即word的长度；
```C++
class Solution {
private:
    bool isExist(int x, int y, int pos, vector<vector<char>>& board, string& word) {
        int len = word.size();
        if (pos == len) {
            return true;
        }
        int m = board.size();
        int n = board[0].size();
        if (x < 0 || x > m - 1 || y < 0 || y > n - 1 || board[x][y] != word[pos]) {
            return false;
        }
        board[x][y] = '*';
        bool ans = isExist(x - 1, y, pos + 1, board, word) 
                || isExist(x + 1, y, pos + 1, board, word)
                || isExist(x, y - 1, pos + 1, board, word)
                || isExist(x, y + 1, pos + 1, board, word);
        board[x][y] = word[pos];
        return ans;
    }
public:
    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size();
        int n = board[0].size();
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (isExist(i, j, 0, board, word)) {
                    return true;
                }
            }
        }
        return false;
    }
};
```