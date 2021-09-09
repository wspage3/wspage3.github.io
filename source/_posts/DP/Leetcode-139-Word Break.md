---
title: Leetcode-139.Word Break
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Hash Table
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[139.Word Break](https://leetcode.com/problems/word-break/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
给定一个长度为size的字符串数组words；
【输出】
判断s能否由words中的单词组成；
对于words[i]，可以使用无限次；
【要求】
无；

### 2、Example
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

<!-- more -->

### 3、Corner Case
1. n > 0
2. words[i].size() > 0
3. size == 0时，输出false；
4. words中无重复元素；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：对于子串s[0...i - 1]，有多少种不同的方案组成；

2. 答案：dp[n]；

3. 初始化
dp[0] = 1；
代表有1种方式组成空串""，那就是：对于words，什么都不选择；

4. 递推式
对于dp[j]来说，是判断s[0...j - 1]有多少种方案组成；
考虑以s[j - 1]结尾的所有子串；

5. Code(https://leetcode.com/submissions/detail/431922055/)
AC
时间复杂度：O(n * n)  
空间复杂度：O(n)
```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& words) {
        int size = words.size();
        if (size == 0) {
            return false;
        }
        
        unordered_map<string, bool> m;
        for (string word : words) {
            m[word] = true;
        }
        
        int n = s.size();
        vector<unsigned int> dp(n + 1, 0);
        dp[0] = 1;
        for (int j = 1; j <= n; j++) {
            int right = j - 1;
            for (int left = 0; left <= right; left++) {
                string str = s.substr(left, right - left + 1);
                if (m.count(str) > 0) {
                    dp[j] += dp[left];
                }
            }
        }
        
        return dp[n];
    }
};
```

6. 时间优化
本题仅仅要求判断s是否能被组成，没有问有多少种方案组成；

7. Code(https://leetcode.com/submissions/detail/431981599/)
AC
时间复杂度：O(n * n)  
空间复杂度：O(n)
```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& words) {
        int size = words.size();
        if (size == 0) {
            return false;
        }
        
        unordered_map<string, bool> m;
        for (string word : words) {
            m[word] = true;
        }
        
        int n = s.size();
        vector<bool> dp(n + 1, false);
        dp[0] = true;
        
        for (int j = 1; j <= n; j++) {
            int right = j - 1;
            for (int left = 0; left <= right; left++) {
                string str = s.substr(left, right - left + 1);
                if (m.count(str) > 0 && dp[left]) {
                    dp[j] = true;
                    break;
                }
            }
        }
        
        return dp[n];
    }
};
```