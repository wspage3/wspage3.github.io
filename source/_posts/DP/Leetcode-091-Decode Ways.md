---
title: Leetcode-091.Decode Ways
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[091.Decode Ways](https://leetcode.com/problems/decode-ways/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
s中元素均为'0'-'9'的数字字符；
【输出】
存在如下一种对字符的编码：
'A' -> 1
'B' -> 2
...
'Z' -> 26
求s一共能解码出多少种不同的情况；
【要求】
无；

### 2、Example
Input: s = "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

Input: s = "0"
Output: 0
Explanation: There is no character that is mapped to a number starting with '0'. We cannot ignore a zero when we face it while decoding. So, each '0' should be part of "10" --> 'J' or "20" --> 'T'.

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 100；
2. s仅包含数字字符；
3. s可能存在前置0；
4. 若存在前置0时，不可能进行解码，输出0；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：子串s[0...i]能解码出多少种不同的情况；

2. 答案：dp[n - 1]；

3. 递推式：
* s[i]独立解码一个字符：dp[i] += dp[i - 1]
* s[i - 1]、s[i]共同解码一个字符：dp[i] += dp[i - 2]

4. Code(https://leetcode.com/submissions/detail/429839202/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    bool isMatch(string& s, int i) {
        if (s[i - 1] <= '0' || s[i - 1] >= '3') {
            return false;
        }
        if (s[i - 1] == '2' && s[i] >= '7') {
            return false;
        }
        return true;
    }
public:
    int numDecodings(string s) {
        int n = s.size();
        // 若有前置0，则不可能进行解码
        if (s[0] == '0') {
            return 0;
        }
        
        vector<int> dp(n);
        dp[0] = 1;
        for (int i = 1; i <= n - 1; i++) {
            dp[i] = 0;
            // s[i]独立解码一个字符
            if (s[i] != '0') {
                dp[i] += dp[i - 1];
            }
            // s[i - 1]、s[i]共同解码一个字符
            if (isMatch(s, i)) {
                dp[i] += i == 1 ? 1 : dp[i - 2];
            }
        }
        return dp[n - 1];
    }
};
```

5. 空间优化

6. Code(https://leetcode.com/submissions/detail/429876075/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
private:
    bool isMatch(string& s, int i) {
        if (s[i - 1] <= '0' || s[i - 1] >= '3') {
            return false;
        }
        if (s[i - 1] == '2' && s[i] >= '7') {
            return false;
        }
        return true;
    }
public:
    int numDecodings(string s) {
        int n = s.size();
        if (s[0] == '0') {
            return 0;
        }
        
        int prepre = 1;
        int pre = 1;
        for (int i = 1; i <= n - 1; i++) {
            int cur = 0;
            if (s[i] != '0') {
                cur += pre;
            }
            if (isMatch(s, i)) {
                cur += prepre;
            }
            prepre = pre;
            pre = cur;
        }
        return pre;
    }
};
```