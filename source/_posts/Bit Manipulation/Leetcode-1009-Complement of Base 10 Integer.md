---
title: Leetcode-1009.Complement of Base 10 Integer
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Bit Manipulation
---

## 一、题意

### 0、题目链接
[1009.Complement of Base 10 Integer](https://leetcode.com/problems/complement-of-base-10-integer/)

### 1、Description
【输入】
给定一个非负整数num；
【输出】
求出num的“补数”；
“补数”规则：将num的二进制形式，每位都取反后，形成的数；
【要求】
无；

### 2、Example
Input: 5
Output: 2
Explanation: 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.

<!-- more -->

### 3、Corner Case
1. num为非负整数；
2. num的二进制形式中，无前置0；例如5，二进制为：101；
3. num保证在32位有符号整数的范围内；即最大为：2147483647；[0111 1111 1111 1111 1111 1111 1111 1111]；

## 二、题解

### 0、思考
1. 本题和476.Number Complement基本一样。唯一的区别在于：本题中num可能为0，特判一下即可。

### 1、Solution-1：Bit Manipulation
1. 同476.Number Complement的思路。

2. Code(https://leetcode.com/submissions/detail/347118770/)
AC
时间复杂度：O(32) = O(1)
空间复杂度：O(1)
```C++
class Solution {
public:
    int bitwiseComplement(int N) {
        if (N == 0) {
            return 1;
        }
        unsigned int mask = ~0;
        while (mask & N) {
            mask = mask << 1;
        }
        return ~mask & ~N;
    }
};
```

