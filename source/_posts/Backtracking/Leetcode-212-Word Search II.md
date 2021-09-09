---
title: Leetcode-212.Word Search II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
- Trie
---

## 一、题意

### 0、题目链接
[212.Word Search II](https://leetcode.com/problems/word-search-ii/)

### 1、Description
【输入】
给定一个m * n的矩阵board；
给定一个长度为k的字符串数组words；
【输出】
把words中满足条件的字符串找出来；
满足条件：board中存在一条路径，路径上的字符连接起来恰好是words[i]；
连接：只能通过上下左右四个方向连接；
【要求】
无；

### 2、Example
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]

<!-- more -->

### 3、Corner Case
1. 1 <= m, n <= 12
2. board、words[i]中仅包含大小写字母；
3. words中无重复单词；
4. 1 <= k <= 30000

## 二、题解

### 1、Solution-1：Brute Force + Backtracking
1. 类似于题目079.Word Search；

2. 外层for循环依次考虑k个word，将满足条件的找出来即可；

3. Code(https://leetcode.com/submissions/detail/424289350/)
AC
时间复杂度：O(k * m * n * 3^len) // k个word；m * n个出发点；每一次，树高len，出度3（除了起点，其它结点只能向三个方向扩展）；  
空间复杂度：O(len) // 递归树最大深度为len，即word的长度；
```C++
class Solution {
private:
    bool helper(int x, int y, int pos, vector<vector<char>>& board, string& word) {
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
        bool ans = helper(x - 1, y, pos + 1, board, word) 
                || helper(x + 1, y, pos + 1, board, word)
                || helper(x, y - 1, pos + 1, board, word)
                || helper(x, y + 1, pos + 1, board, word);
        board[x][y] = word[pos];
        return ans;
    }
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        vector<string> ans;
        int m = board.size();
        int n = board[0].size();
        for (string word : words) {
            bool ok = false;
            for (int i = 0; i <= m - 1; i++) {
                for (int j = 0; j <= n - 1; j++) {
                    if (helper(i, j, 0, board, word) == true) {
                        ans.push_back(word);
                        ok = true;
                        break;
                    }
                }
                if (ok) {
                    break;
                }
            }
        }
        return ans;
    }
};
```

### 2、Solution-2：Trie + Backtracking
1. 将words中的所有单词，构造一棵字典树trie；

2. 从m * n个起点出发，开始搜索：helper(i, j, board, trie, s, ans);
含义：
从(i, j)出发；
搜索目标：以trie为根结点的字典树上的所有单词，若找到某个：继续搜索，不回溯；
搜索路径：路径上的字符依次保存到字符串s，方便将其加入答案ans中；

3. 去重策略：
假设从起点(i1, j1)出发，可以搜索到路径"abc"，此时将其加入答案ans中；
同时从另外一个起点(i2, j2)出发，同样搜索到了"abc"，则此时不能继续将其加入ans中；
方法：加入ans的时候，将字典树中最后一个字符对应的结点的isEnd标记修改为false；

3. Code(https://leetcode.com/submissions/detail/425658143/)
AC
时间复杂度：O(m * n * 3^len) // m * n个出发点；每一次，树高len，出度3（除了起点，其它结点只能向三个方向扩展）；  
空间复杂度：O(len) // 递归树最大深度为len，即word的长度；
```C++
class Trie {
public:
    bool isEnd;
    Trie* next[26];
    Trie() {
        isEnd = false;
        for (int i = 0; i <= 25; i++) {
            next[i] = NULL;
        }
    }
    void insert(string word) {
        Trie* cur = this;
        for (char c : word) {
            c -= 'a';
            if (cur->next[c] == NULL) {
                cur->next[c] = new Trie();
            }
            cur = cur->next[c];
        }
        cur->isEnd = true;
    }
};
 
class Solution {
private:
    void helper(int x, int y, vector<vector<char>>& board, Trie* node, string& s, vector<string>& ans) {
        int m = board.size();
        int n = board[0].size();
        // 不是一个合法的起点(board[x][y])；
        if (x < 0 || x > m - 1 || y < 0 || y > n - 1 || board[x][y] == '*') {
            return;
        }
        
        char c = board[x][y];
        // 是一个合法的起点；
        // 但是从该起点出发，不可能匹配到目标字符串(以node为根的字典树)中的任意一个；
        if (node->next[c - 'a'] == NULL) {
            return;
        }
        
        // 随着不断的搜索，将当前搜索路径上的字符串保存到s；
        s.push_back(c);
        if (node->next[c - 'a']->isEnd == true) {
            // 找到一个解：
            // 此时将这个解的isEnd标记修改为false，目的：防止从其它路径搜索过来时，继续将这个解加入答案中；
            // 此时不能回溯，原因：假设当前解为"abc"，后面还有可能出现"abcd"、"abce"等可行解；
            ans.push_back(s);
            node->next[c - 'a']->isEnd = false;
        }
        board[x][y] = '*';
        helper(x - 1, y, board, node->next[c - 'a'], s, ans);
        helper(x + 1, y, board, node->next[c - 'a'], s, ans);
        helper(x, y - 1, board, node->next[c - 'a'], s, ans);
        helper(x, y + 1, board, node->next[c - 'a'], s, ans);
        board[x][y] = c;
        s.pop_back();
    }
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        Trie* trie = new Trie();
        for (string word : words) {
            trie->insert(word);
        }
        
        vector<string> ans;
        int m = board.size();
        int n = board[0].size();
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                string s;
                helper(i, j, board, trie, s, ans);
            }
        }
        return ans;
    }
};
```