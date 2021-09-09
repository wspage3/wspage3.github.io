---
title: Leetcode-140.Word Break II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Hash Table
- Dynamic Programming
- Backtracking
---

## 一、题意

### 0、题目链接
[140.Word Break II](https://leetcode.com/problems/word-break-ii/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
给定一个长度为size的字符串数组words；
【输出】
求出s由words中的单词组成的所有方案，每种方案保存在一个字符串中；
对于words[i]，可以使用无限次；
【要求】
无；

### 2、Example
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]

<!-- more -->

### 3、Corner Case
1. n > 0
2. words[i].size() > 0
3. size == 0时，输出空；
4. words中无重复元素；

## 二、题解

### 1、Solution-1：Dynamic Programming + Backtracking
1. dp[j]表示：对于子串s[0...j - 1]，有多少种不同的方案组成；

2. 同时，在求dp[j]的过程中，将与s[j - 1]所匹配的left索引都保存在v[j - 1]数组中；

3. 根据v，进行回溯，找出所有路径；

4. Code(https://leetcode.com/submissions/detail/431934395/)
AC
时间复杂度：O(n * n)  
空间复杂度：O(n)
```C++
class Solution {
private:
    void backtracking(int start, vector<vector<int>>& v, string& s, string& sub, vector<string>& ans) {
        if (start == -1) {
            string temp = sub;
            temp.pop_back();
            reverse(temp.begin(), temp.end());
            ans.push_back(temp);
            return;
        }
        
        for (int idx : v[start]) {
            for (int i = start; i >= idx; i--) {
                sub.push_back(s[i]);
            }
            sub.push_back(' ');
            backtracking(idx - 1, v, s, sub, ans);
            sub.pop_back();
            for (int i = start; i >= idx; i--) {
                sub.pop_back();
            }
        }
    }
public:
    vector<string> wordBreak(string s, vector<string>& words) {
        int size = words.size();
        if (size == 0) {
            return {};
        }
        
        unordered_map<string, bool> m;
        for (string word : words) {
            m[word] = true;
        }
        
        int n = s.size();
        vector<vector<int>> v(n);
        vector<unsigned int> dp(n + 1, 0);
        dp[0] = 1;
        for (int j = 1; j <= n; j++) {
            int right = j - 1;
            for (int left = 0; left <= right; left++) {
                string str = s.substr(left, right - left + 1);
                if (m.count(str) > 0) {
                    dp[j] += dp[left];
                    v[right].push_back(left);
                }
            }
        }
        
        if (dp[n] == 0) {
            return {};
        }
        
        vector<string> ans;
        string sub;
        backtracking(n - 1, v, s, sub, ans);
        return ans;
    }
};
```