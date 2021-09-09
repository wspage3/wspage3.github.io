---
title: Leetcode-125.Valid Palindrome
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- String
- Two Pointers
---

## 一、题意

### 0、题目链接
[125.Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
s中的字符均为可显示的ASCII字符（32-126）；
控制字符（0-31，127）；
【输出】
仅仅考虑s中的字母字符、数字字符；同时忽略字母字符大小写的情况下；判断s是否为回文串；
【要求】
无；

### 2、Example
Input: "A man, a plan, a canal: Panama"
Output: true

<!-- more -->

### 3、Corner Case
1. n == 0时，认为是回文串；

## 二、题解

### 1、Solution-1：Two Pointers
1. 指针l从0开始，向右走；

2. 指针r从n - 1开始，向左走；

3. Code(https://leetcode.com/submissions/detail/427073039/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isPalindrome(string s) {
        int n = s.size();
        int l = 0;
        int r = n - 1;
        while (l < r) {
            // isalnum(c)：当c为大小写字符或数字字符时，返回非零int，否则返回0；
            if (isalnum(s[l]) == 0) {
                l++;
                continue;
            }
            if (isalnum(s[r]) == 0) {
                r--;
                continue;
            }
            
            // toupper(c)：将小写字符c转换为大写字符；若c不是小写字符，则返回c；
            if (toupper(s[l]) != toupper(s[r])) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
};
```