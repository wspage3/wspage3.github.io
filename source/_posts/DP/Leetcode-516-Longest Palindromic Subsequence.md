---
title: Leetcode-516.Longest Palindromic Subsequence
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[516.Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
【输出】
求出s所有的子序列中，最长的回文子序列，输出这个长度；
【要求】
无；

### 2、Example
Input: "bbbab"
Output: 4
One possible longest palindromic subsequence is "bbbb".

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 1000
2. n == 1时，输出1；

## 二、题解

### 1、Solution-1：Brute Force
1. 长度为n的字符串s，非空子序列有O(2 ^ n)个；

### 2、Solution-2：Dynamic Programming
1. dp[i][j]表示：范围s[i...j]中，最长的回文子序列的长度；

2. 答案
max(dp[0][n - 1])；

3. 初始化
dp为n行n列的矩阵；
i > j时，不可能构成子序列，dp[i][j] = 0；
i = j时，dp[i][j] = 1；
i < j时，考虑是否可以进行状态转移；

4. 递推式
当i < j时，才需要考虑递推式；
* j = i + 1
若s[i]、s[j]相同，则：dp[i][j] = 2；
否则，dp[i][j] = 1；
* j > i + 1
若s[i]、s[j]相同，则：dp[i][j] = 2 + dp[i + 1][j - 1]；
否则，dp[i][j] = max(dp[i][j - 1], dp[i + 1][j])；
取较大值即可；

5. 计算顺序
计算第i行，需要第i + 1行的数据；
所以i从下到上遍历；

6. Code(https://leetcode.com/submissions/detail/431170559/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n * n)
```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        if (n == 1) {
            return 1;
        }
        
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j <= n - 1; j++) {
                if (j == i) {
                    dp[i][j] = 1;
                } else if (j == i + 1) {
                    dp[i][j] = s[i] == s[j] ? 2 : 1;
                } else {
                    dp[i][j] = s[i] == s[j] ? dp[i + 1][j - 1] + 2 : max(dp[i][j - 1], dp[i + 1][j]);
                }
            }
        }
        
        return dp[0][n - 1];
    }
};
```

7. 空间优化

8. Code(https://leetcode.com/submissions/detail/431172615/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        if (n == 1) {
            return 1;
        }
        
        vector<int> down(n, 0);
        down[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            vector<int> up(n, 0);
            for (int j = i; j <= n - 1; j++) {
                if (j == i) {
                    up[j] = 1;
                } else if (j == i + 1) {
                    up[j] = s[i] == s[j] ? 2 : 1;
                } else {
                    up[j] = s[i] == s[j] ? down[j - 1] + 2 : max(up[j - 1], down[j]);
                }
            }
            up.swap(down);
        }
        
        return down[n - 1];
    }
};
```