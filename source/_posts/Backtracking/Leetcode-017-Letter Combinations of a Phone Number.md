---
title: Leetcode-017.Letter Combinations of a Phone Number
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
[017.Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

### 1、Description
【输入】
给定一个长度为n的字符串digits：代表着用户在9键键盘上的一个按键操作；
digits中均为'2'-'9'的数字字符；
【输出】
将digits所能表示的所有文本输出，求出来；
每个文本输出是一个字符串，最终输出一个字符串数组；
【要求】
无；

### 2、Example
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 4；
2. digits合法：s中均为'2'-'9'的数字字符；
3. 若n == 0，输出空数组；

## 二、题解

### 1、Solution-1：Backtracking
1. 从digits[0]这个位置开始，一直到digits[n - 1]结束。
代表：digits中的每个字符都不重复不遗漏的考虑到了；

2. 合法答案：start == n；

3. Code(https://leetcode.com/submissions/detail/426736512/)
AC
// 每个结点的“度数”：最大为4
// 递归树的高度为：n
时间复杂度：O((4 ^ n) * n) // 对于4^n种不同的情况，每一种情况需要组装一个长度为n的字符串sub；
空间复杂度：O(n) 
```C++
class Solution {
private:
    vector<vector<char>> v = {{'a', 'b', 'c'},
                              {'d', 'e', 'f'}, 
                              {'g', 'h', 'i'}, 
                              {'j', 'k', 'l'}, 
                              {'m', 'n', 'o'}, 
                              {'p', 'q', 'r', 's'}, 
                              {'t', 'u', 'v'}, 
                              {'w', 'x', 'y', 'z'}};
    void backtracking(int start, string& digits, string& sub, vector<string>& ans) {
        int n = digits.size();
        if (start == n) {
            ans.push_back(sub);
            return;
        }
        
        int size = v[digits[start] - '2'].size();
        for (int i = 0; i <= size - 1; i++) {
            sub.push_back(v[digits[start] - '2'][i]);
            backtracking(start + 1, digits, sub, ans);
            sub.pop_back();
        }
    }
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        int n = digits.size();
        if (n == 0) {
            return ans;
        }
        
        string sub;
        backtracking(0, digits, sub, ans);
        return ans;
    }
};
```