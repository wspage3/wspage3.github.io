---
title: 268.Missing Number
---

## 一、题意
题目链接: [268.Missing Number](https://leetcode.com/problems/missing-number/)
### 1.Description
【输入】
给定一个长度为n的整数数组。
其中所有的元素取值都是区间[0, n]的，且没有重复的元素。
显然，[0, n]一共有(n + 1)个元素，而nums中只有n个，找出这个缺失的元素。
【输出】
找出这个缺失的数字。

### 2.Example
Input: [9,6,4,2,3,5,7,0,1]
Output: 8

### 3.Corner Case
1.空间复杂度要求为O(1)。
2.时间复杂度要求O(n)。

## 二、题解
### Solution-1：Accumulate 
1.[0, n]中所有数字的和为[1, n] = n * (n + 1) / 2

2.我们遍历nums中的数字，累加一个和。
然后用上面的公式计算出来的和减去nums中累加的和，答案就是缺失的元素。

时间复杂度：O(n)
空间复杂度：O(1)

2.Code(https://leetcode.com/submissions/detail/285827066/)
Code(https://leetcode.com/submissions/detail/285849852/)

3.缺点：accumulate可以会导致整数溢出，存在局限。

### Solution-2：XOR
1.任意一个数 异或 它自己 == 0
  任意一个数 异或 0     == 它自己
  异或满足交换律，结合率

2.nums中有n个数字，且都在范围[0, n]内，且都不重复。
我们把nums中所有的数字都异或起来。记为a。
然后把[0, n]所有的数字都异或起来。记为b。
此时所有参与异或的数字一共有 n + (n + 1)个。
假设missing number为x。则上述n + (n + 1)个数字只有一个x，其它数字都是出现了两次。
则 a ^ b == x。

时间复杂度：O(n)
空间复杂度：O(1)

2.Code(https://leetcode.com/submissions/detail/285850682/)
