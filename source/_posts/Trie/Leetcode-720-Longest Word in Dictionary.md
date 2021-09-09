---
title: Leetcode-720.Longest Word in Dictionary
categories: 
- [Leetcode]
- [数据结构与算法] 
tags: 
- String
- Trie
- DFS
---

## 一、题意

### 0、题目链接
[720.Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

### 1、Description
【输入】
给定一个长度为n的字符串数组words；
【输出】
求出words中，满足条件的字符串中，长度最大的那个；
如果存在长度相同，则选择字典序小的那个字符串；
【要求】
无；

### 2、Example
Input: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
Output: "apple"
Explanation: 
Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".

<!-- more -->

### 3、Corner Case
1. 假定所有的字符串中仅包含：小写字母，即'a'~'z'；
2. 1 <= n <= 1000；
3. 1 <= len <= 30；
4. 考虑n == 1时的情况；
5. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. 满足条件：
若字符串s[0, n - 1]满足条件，则s[0, n - 2]、s[0, n - 3]、...、s[0]必须都出现在words中；

2. 注意：若s的长度为1，则认为其满足条件；

3. 求上述满足条件的所有字符串中，长度最大的那个（若长度相同，选择字典序小的）。

4. 若找不到满足条件的字符串，返回""；

### 1、Solution-1：Trie + DFS
1. 将words中的n个字符串，依次插入到Trie中。

2. 从root结点开始，遍历字典树（DFS/Backtracking），求出答案；

3. Code(https://leetcode.com/submissions/detail/425702166/)
AC
时间复杂度：O(n * Len)
空间复杂度：O(n * Len)
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
    void dfs(Trie* root, string& cur, string& ans, int curDepth) {
        if (root == NULL) {
            return;
        }
        if (curDepth > ans.size()) {
            ans = cur;
        }
        for (int i = 0; i <= 25; i++) {
            Trie* son = root->next[i];
            if (son == NULL || son->isEnd == false) {
                continue;
            }
            cur.push_back('a' + i);
            dfs(son, cur, ans, curDepth + 1);
            cur.pop_back();
        }
    }
public:
    string longestWord(vector<string>& words) {
        Trie* trie = new Trie();
        for (string word : words) {
            trie->insert(word);
        }
        
        string ans;
        string cur;
        dfs(trie, cur, ans, 0);
        return ans;
    }
};
```

