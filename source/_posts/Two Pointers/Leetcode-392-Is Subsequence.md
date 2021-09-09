---
title: Leetcode-392.Is Subsequence
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
- Two Pointers
---

## 一、题意

### 0、题目链接
[392.Is Subsequence](https://leetcode.com/problems/is-subsequence/)

### 1、Description
【输入】
给定两个字符串s、t，长度分别为ns、nt；
【输出】
求s是否为t的一个子序列；
【要求】
无；

### 2、Example
Input: s = "abc", t = "ahbgdc"
Output: true

Input: s = "axc", t = "ahbgdc"
Output: false

<!-- more -->

### 3、Corner Case
1. 0 <= ns <= 100
2. 0 <= nt <= 10000
3. 当ns = 0时，输出true；
4. 当ns > nt时，输出false；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. Leetcode-1143-Longest Common Subsequence
我们以O(n * n)的时间复杂度，计算s和t的最长公共子序列长度；

2. 本题，同样直接计算s和t的最长公共子序列长度，记为l：
* l = ns：s是t的子序列
* l < ns：s不是t的子序列
* l > ns：不可能；

3. Code(https://leetcode.com/submissions/detail/431297129/)
AC
时间复杂度：O(ns * nt)
空间复杂度：O(ns * nt)
```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int ns = s.size();
        int nt = t.size();
        if (ns == 0) {
            return true;
        }
        if (ns > nt) {
            return false;
        }
        
        vector<vector<int>> dp(nt + 1, vector<int>(ns + 1, 0));
        for (int i = 1; i <= nt; i++) {
            for (int j = 1; j <= ns; j++) {
                if (t[i - 1] == s[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        
        return dp[nt][ns] == ns;
        
    }
};
```

4. 空间优化
时间复杂度：O(ns * nt)
空间复杂度：O(min(ns, nt))

### 2、Solution-2：Two Pointers
1. 依次判断s中的每个字符，是否均在t中出现；

2. Code(https://leetcode.com/submissions/detail/431301630/)
AC
时间复杂度：O(nt)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int ns = s.size();
        int nt = t.size();
        if (ns == 0) {
            return true;
        }
        if (ns > nt) {
            return false;
        }
        
        int i = 0;
        int j = 0;
        while (i <= ns - 1 && j <= nt - 1) {
            if (s[i] == t[j]) {
                i++;
            }
            j++;
        }
        
        return i == ns;        
    }
};
```

3. Code(https://leetcode.com/submissions/detail/431306947/)
AC
时间复杂度：O(nt)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int ns = s.size();
        int nt = t.size();
        if (ns == 0) {
            return true;
        }
        if (ns > nt) {
            return false;
        }
        
        int pos = 0;
        for(char c : s) {
            pos = t.find(c, pos);
            if (pos == string::npos) {
                return false;
            }
            pos++;
        }
        
        return true;
    }
};
```
