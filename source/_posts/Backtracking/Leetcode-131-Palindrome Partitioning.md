---
title: Leetcode-131.Palindrome Partitioning
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
- String
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[131.Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
【输出】
将s拆分成若干个子串，每个子串均为回文串；
求出所有的拆分情况；
每种情况保存在一个一维字符串数组，最终输出一个二维字符串数组；
【要求】
无；

### 2、Example
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]

<!-- more -->

### 3、Corner Case
1. n == 0时，输出空；

## 二、题解

### 1、Solution-1：Backtracking
1. 从s[0]这个位置开始，一直到s[n - 1]结束。
代表：s中的每个字符都不重复不遗漏的考虑到了；

2. 合法答案：start == n；

3. Code(https://leetcode.com/submissions/detail/427019326/)
AC
// 每个结点的“度数”：最小为1，最大为n
// 递归树的高度为：最小为1，最大为n
// n = 3时，有4种拆分方法
// n = 4时，有8种拆分方法
时间复杂度：O((2 ^ n) * n) // 对于2^n种不同的情况，每一种情况需要组装一个长度为n的字符串sub；
空间复杂度：O(n) 
```C++
class Solution {
private:
    // 判断s[start, end]是否为回文串
    bool isPalindrome(string& s, int l, int r) {
        // 1. l > r，invalid input
        // 2. l == r，return true
        // 3. l < r，check
        if (l > r) {
            return false; // invalid input
        }
        while (l < r) {
            if (s[l] != s[r]) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
    void backtracking(int start, string& s, vector<string>& sub, vector<vector<string>>& ans) {
        int n = s.size();
        if (start == n) {
            ans.push_back(sub);
            return;
        }
        for (int end = start; end <= n - 1; end++) {
            if (isPalindrome(s, start, end) == false) {
                continue;    
            }
            sub.push_back(s.substr(start, end - start + 1));
            backtracking(end + 1, s, sub, ans);
            sub.pop_back();
        }
    }
public:
    vector<vector<string>> partition(string s) {
        vector<string> sub;
        vector<vector<string>> ans;
        backtracking(0, s, sub, ans);
        return ans;
    }
};
```

### 2、Solution-2：Backtracking + Dynamic Programming
1. 判断子串s[start, end]是否为回文串时，使用动态规划来优化；