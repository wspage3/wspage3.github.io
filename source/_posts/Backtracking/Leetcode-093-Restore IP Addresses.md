---
title: Leetcode-093.Restore IP Addresses
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
- String
---

## 一、题意

### 0、题目链接
[093.Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
s中均为'0'-'9'的数字字符；
【输出】
将s所能表示的所有ip地址，求出来；
每个ip地址是一个字符串，最终输出一个字符串数组；
ip地址格式：4个数字；每个数字中间有一个'.'；每个数字取值范围0-255；每个数字不能有前置的0，如012；
【要求】
无；

### 2、Example
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
Input: s = "0000"
Output: ["0.0.0.0"]

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 3000；
2. s合法：s中均为'0'-'9'的数字字符；
3. 若n < 4 || n > 12，则一定不可能表示ip地址；

## 二、题解

### 1、Solution-1：Backtracking
1. 从s[0]这个位置开始，一直到s[n - 1]结束。
代表：s中的每个字符都不重复不遗漏的考虑到了；

2. 合法答案：start == n && parts == 4；

3. Code(https://leetcode.com/submissions/detail/426708112/)
AC
// 每个结点的“度数”：最大为3
// 递归树的高度为：h，h最大为4
时间复杂度：O((3 ^ 4) * n) // 对于3^4种不同的情况，每一种情况需要组装一个长度为n的字符串sub；
空间复杂度：O(n) 
```C++
class Solution {
private:
    bool isOk(string& num) {
        // 1. check leading zero
        if (num[0] == '0' && num.size() > 1) {
            return false;
        }
        // 2. check greater than 255
        int x = stoi(num);
        return x <= 255;
    }
    void backtracking(int start, string& s, string& sub, int& parts, vector<string>& ans) {
        int n = s.size();
        if (start == n) {
            if (parts == 4) {
                sub.pop_back();
                ans.push_back(sub);
            }
            return;
        }
        for (int i = start; i <= min(n - 1, start + 2); i++) {
            string cur = s.substr(start, i - start + 1);
            if (isOk(cur) == false) {
                continue;
            }
            string old = sub;
            sub += cur + ".";
            parts++;
            backtracking(i + 1, s, sub, parts, ans);
            parts--;
            sub = old;
        }
    }
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> ans;
        int n = s.size();
        if (n < 4 || n > 12) {
            return ans;
        }
        string sub;
        int parts = 0;
        backtracking(0, s, sub, parts, ans);
        return ans;
    }
};
```

### 2、Solution-2：Brute Force
1. Code(https://leetcode.com/submissions/detail/426716235/)
AC
时间复杂度：O((3 ^ 4) * n) // 对于3^4种不同的情况，每一种情况需要组装一个长度为n的字符串sub；
空间复杂度：O(n) 
```C++
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> ans;
        int n = s.size();
        if (n > 12 || n < 4) {
            return ans;
        }
        
        string sub;
        for (int a = 1; a <= 3; a++) {
            for (int b = 1; b <= 3; b++) {
                for (int c = 1; c <= 3; c++) {
                    for (int d = 1; d <= 3; d++) {
                        if (a + b + c + d == n) {
                            int A = stoi(s.substr(0, a));
                            int B = stoi(s.substr(a, b));
                            int C = stoi(s.substr(a + b, c));
                            int D = stoi(s.substr(a + b + c, d));
                            // 判断每个数字是否都不大于255
                            if (A <= 255 && B <= 255 && C <= 255 && D <= 255) {
                                sub = to_string(A) + "."
                                     + to_string(B) + "."
                                     + to_string(C) + "."
                                     + to_string(D);
                                // 判断是否有一个数字，其有前置的0
                                // 例如s = "01023"
                                // 情况1：sub = "0.1.0.23"；
                                // 情况2：sub = "0.1.2.3"；舍弃；
                                if (sub.size() == n + 3) {
                                    ans.push_back(sub); 
                                }
                            }
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```