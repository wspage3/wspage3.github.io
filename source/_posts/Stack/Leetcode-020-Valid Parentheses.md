---
title: Leetcode-020.Valid Parentheses
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Stack
---

## 一、题意

### 0、题目链接
[020.Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
【输出】
判断s是否为一个合法的括号表达式；
【要求】
无；

### 2、Example
Input: s = "()[]{}"
Output: true
Input: s = "(]"
Output: false

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 10000
2. s合法：仅包含'('、')'、'{'、'}'、'['、']'；

## 二、题解

### 1、Solution-1：Stack
1. 特判：若n为奇数，则一定不合法；

2. 从左到右遍历s，对于字符c：
* c为左括号：将其放入栈中；
* c为右括号：判断和栈顶元素是否匹配，不匹配说明s非法，匹配的话抛出栈顶元素；

3. 若s合法，则最终栈一定为空；

4. Code(https://leetcode.com/submissions/detail/428886085/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    bool isMatch(char c, char top) {
        if (c == ')' && top == '('
         || c == ']' && top == '[' 
         || c == '}' && top == '{') {
            return true;
        }
        return false;
    }
public:
    bool isValid(string s) {
        int n = s.size();
        if (n % 2 == 1) {
            return false;
        }
        
        stack<char> stack;
        for (char c : s) {
            if (c == '(' || c == '[' || c == '{') {
                stack.push(c);
            } else {
                if (stack.empty() || isMatch(c, stack.top()) == false) {
                    return false;
                }
                stack.pop();
            }
        }
        return stack.empty();
    }
};
```

5. Code(https://leetcode.com/submissions/detail/428888912/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    bool isValid(string s) {
        int n = s.size();
        if (n % 2 == 1) {
            return false;
        }
        
        stack<char> stack;
        for (char c : s) {
            if (c == '(') {
                stack.push(')');
            } else if (c == '{') {
                stack.push('}');
            } else if (c == '[') {
                stack.push(']');
            } else {
                if (stack.empty() || c != stack.top()) {
                    return false;
                }
                stack.pop();
            }
               
        }
        return stack.empty();
    }
};
```