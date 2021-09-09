---
title: Leetcode-009.Palindrome Number
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Math
- Two Pointers
---

## 一、题意

### 0、题目链接
[009.Palindrome Number](https://leetcode.com/problems/palindrome-number/)

### 1、Description
【输入】
给定一个整数x；
【输出】
判断x是否为回文数；
回文数：从左到右读，与从右到左读，得到的数字是相同的；
【要求】
无；

### 2、Example
Input: x = 121
Output: true
Input: x = -121
Output: false

<!-- more -->

### 3、Corner Case
1. INT_MIN <= x <= INT_MAX

## 二、题解

### 1、Solution-1：Two Pointers
1. 将x转化为字符串s，判断s是否为回文串；

2. Code(https://leetcode.com/submissions/detail/428584803/)
AC
l：字符串s的长度，最大为10；
时间复杂度：O(l) 
空间复杂度：O(l)
```C++
class Solution {
private:
    bool helper(string& s) {
        int n = s.size();
        int l = 0;
        int r = n - 1;
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
    bool isPalindrome(int x) {
        string s = to_string(x);
        return helper(s);
    }
};
```

### 2、Solution-2：Math
1. 从右到左读，保存到y中。

2. Code(https://leetcode.com/submissions/detail/428589712/)
AC
l：字符串s的长度，最大为10；
时间复杂度：O(l) 
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0 || (x > 0 && x % 10 == 0)) {
            return false;
        }
        
        int _x = x;
        unsigned int y = 0;
        while (x) {
            y = y * 10 + x % 10;
            x /= 10;
        }
        return y == _x;
    }
};
```