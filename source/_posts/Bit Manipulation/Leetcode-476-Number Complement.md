---
title: Leetcode-476.Number Complement
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Bit Manipulation
---

## 一、题意

### 0、题目链接
[476.Number Complement](https://leetcode.com/problems/number-complement/)

### 1、Description
【输入】
给定一个正整数num；
【输出】
求出num的“补数”；
“补数”规则：将num的二进制形式，每位都取反后，形成的数；
【要求】
无；

### 2、Example
Input: num = 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.

<!-- more -->

### 3、Corner Case
1. num为正整数；
2. num的二进制形式中，无前置0；例如5，二进制为：101；
3. num保证在32位有符号整数的范围内；即最大为：2147483647；[0111 1111 1111 1111 1111 1111 1111 1111]；

## 二、题解

### 1、Solution-1：Bit Manipulation
1. ~操作符：按位取反。

2. 若对num直接进行按位取反，则会导致前置0均转化为1。
例如：在int为32位大小的环境中，5的存储为：[0000 0000 0000 0000 0000 0000 0000 0101]。
若直接进行按位取反，会变成：[1111 1111 1111 1111 1111 1111 1111 1010]，即-6。

3. 负数的补码：写出对应正数的二进制形式；再逐位取反；再加1；
例如：求-6的补码形式。
[0000 0000 0000 0000 0000 0000 0000 0110]
[1111 1111 1111 1111 1111 1111 1111 1001]
[1111 1111 1111 1111 1111 1111 1111 1010]

4. 在本题中，我们不希望考虑num的二进制形式中的前置0。

5. 继续考虑求5的“补数”：
[1111 1111 1111 1111 1111 1111 1111 1111]-- mask的初始值
[0000 0000 0000 0000 0000 0000 0000 0101]-- 5的二进制
[1111 1111 1111 1111 1111 1111 1111 1000]-- mask的最终值

6. 
[1111 1111 1111 1111 1111 1111 1111 1010]-- ~5的二进制
[0000 0000 0000 0000 0000 0000 0000 0111]-- ~mask的二进制
[0000 0000 0000 0000 0000 0000 0000 0010]-- 最终结果，十进制表示为2；

7. Code(https://leetcode.com/submissions/detail/342112377/)
AC
时间复杂度：O(32) = O(1)
空间复杂度：O(1)
```C++
class Solution {
public:
    int findComplement(int num) {
        unsigned int mask = ~0;
        while (mask & num) {
            mask = mask << 1;
        }
        return ~mask & ~num;
        
    }
};
```

