---
title: Leetcode-680.Valid Palindrome II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- String
- Two Pointers
---

## 一、题意

### 0、题目链接
[680.Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
【输出】
在最多删除一个字符的情况下，判断s是否为回文串；
【要求】
无；

### 2、Example
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.

<!-- more -->

### 3、Corner Case
1. n >= 1；
2. s中字符均为小写字母字符；

## 二、题解

### 1、Solution-1：Two Pointers
1. 指针l从0开始，向右走；

2. 指针r从n - 1开始，向左走；

3. 当s[l]和s[r]第一次不匹配的时候，停止；

4. 记子串s[l, r]的长度为len；
若len为奇数，例如：s[l],s[l + 1],s[l + 2],s[l + 3],s[r]
若len为偶数，例如：s[l],s[l + 1],s[l + 2],s[r]
假设我们删除s[l + 1, r - 1]范围内的任一元素，可以发现：s[l]还是需要和s[r]进行比较。
而已知s[l] != s[r]，所以删除上述元素，对构成回文串无任何帮助；
所以，我们发现不匹配的时候，需要删除s[l]或者s[r]；

5. Code(https://leetcode.com/submissions/detail/427440569/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
private:
    bool isPalindrome(string& s, int l, int r) {
        while (l < r) {
            if (s[l] != s[r]) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
public:
    bool validPalindrome(string s) {
        int n = s.size();
        int l = 0;
        int r = n - 1;
        while (l < r) {
            if (s[l] == s[r]) {
                l++;
                r--;
                continue;
            } else {
                return isPalindrome(s, l + 1, r) || isPalindrome(s, l, r - 1);
            }
        }
        return true;
    }
};
```