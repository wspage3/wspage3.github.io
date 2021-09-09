---
title: Leetcode-1143.Longest Common Subsequence
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[1143.Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

### 1、Description
【输入】
给定两个字符串s、t，长度分别为ns、nt；
【输出】
求s和t的最长公共子序列的长度；
LCS（Longest Common Subsequence）；
【要求】
无；

### 2、Example
Input: s = "abcde", t = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.

<!-- more -->

### 3、Corner Case
1. 1 <= ns、nt <= 1000

## 二、题解

### 0、思考
1. 经典问题LCS：涉及到两个字符串的动态规划；

2. 参考(https://www.youtube.com/watch?v=NnD96abizww)

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：子串s[0...i - 1]和子串t[0...j - 1]的LCS的长度；

2. 答案：dp[ns][nt]；

3. 初始化
dp为ns + 1行、nt + 1列的矩阵；
第一行、第一列初始化为0；
i从1开始，到ns结束、j从1开始，到nt结束，计算；

4. 递推式
* s[i - 1] == t[j - 1]
说明这个字符一定在子串s[0...i - 1]和子串t[0...j - 1]的LCS中；
dp[i][j] = dp[i - 1][j - 1] + 1;
* s[i - 1] == t[j - 1]
两个字符至少有一个不在子串s[0...i - 1]和子串t[0...j - 1]的LCS中；
s[i - 1]不在LCS中：取上方元素dp[i - 1][j]；
t[j - 1]不在LCS中：取左边元素dp[i][j - 1]；
s[i - 1]、t[j - 1]不在LCS中：取左上方元素dp[i - 1][j - 1]；
注意：由于我们是求max值，所以上述三部分有重复不影响max值，但不允许有遗漏；
同时可以发现，dp[i - 1][j]、dp[i][j - 1]均大于等于dp[i - 1][j - 1]，所以求两者的max值即可；

5. 计算顺序
计算dp[i][j]，可能需要dp[i - 1][j - 1]、dp[i][j - 1]、dp[i - 1][j]；

6. Code(https://leetcode.com/submissions/detail/431213446/)
AC
时间复杂度：O(ns * nt)
空间复杂度：O(ns * nt)
```C++
class Solution {
public:
    int longestCommonSubsequence(string s, string t) {
        int ns = s.size();
        int nt = t.size();
        if (ns < nt) {
            return longestCommonSubsequence(t, s);
        }
        
        vector<vector<int>> dp(ns + 1, vector<int>(nt + 1, 0));
        for (int i = 1; i <= ns; i++) {
            for (int j = 1; j <= nt; j++) {
                if (s[i - 1] == t[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        
        return dp[ns][nt];
    }
};
```

7. 空间优化
* 二维空间降为一维空间；
* 选择min(ns, nt)作为一维空间长度；

8. Code(https://leetcode.com/submissions/detail/431214737/)
AC
时间复杂度：O(ns * nt)
空间复杂度：O(min(ns, nt))
```C++
class Solution {
public:
    int longestCommonSubsequence(string s, string t) {
        int ns = s.size();
        int nt = t.size();
        if (ns < nt) {
            return longestCommonSubsequence(t, s);
        }
        
        vector<int> pre(nt + 1, 0);
        for (int i = 1; i <= ns; i++) {
            vector<int> cur(nt + 1, 0);
            for (int j = 1; j <= nt; j++) {
                if (s[i - 1] == t[j - 1]) {
                    cur[j] = pre[j - 1] + 1;
                } else {
                    cur[j] = max(cur[j - 1], pre[j]);
                }
            }
            swap(pre, cur);
        }
        
        return pre[nt];
    }
};
```