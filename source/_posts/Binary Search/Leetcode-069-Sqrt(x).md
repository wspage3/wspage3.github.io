---
title: Leetcode-069.Sqrt(x)
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Search
---

## 一、题意

### 0、题目链接
[069.Sqrt(x)](https://leetcode.com/problems/sqrtx/)

### 1、Description
【输入】
给定一个非负整数x；
【输出】
计算x的平方根，答案保留整数部分即可，小数舍弃。
【要求】
无；

### 2、Example
Input: 4
Output: 2

Input: 8
Output: 2

<!-- more -->

### 3、Corner Case
1. x == 0的时候，答案为0；
2. x == 1的时候，答案为1；
3. 考虑x >= 2的情况；

## 二、题解

### 0、思考
1. 使用二分法，“猜”答案即可。

### 1、Solution-1：Binary Search
1. 处理x == 0 || x == 1的情况；

2. 在[1, x)上搜索，找到最后一个满足条件的数字target。二分模板-3。

3. 条件：target * target <= x；

4. Code(https://leetcode.com/submissions/detail/331746416/)
AC
时间复杂度：O(log x)
空间复杂度：O(1)
```C++
class Solution {
public:
    int mySqrt(int x) {
        if (x == 0) {
            return 0;
        }
        if (x == 1) {
            return 1;
        }
        // 考虑x >= 2的情况：
        int left = 1;
        int right = x;
        // 注意：按照二分模板-3，我们应该在[left, right + 1)上进行搜索。其中[left, right]是target所有可能的取值空间。
        // 由于x >= 2，所以：left最小为1；right最大为x - 1。
        // 所以我们在：[1, x - 1 + 1)中搜索。避免了x为INT_MAX导致overflow的情况。
        while (left < right) {
            int mid = left + (right - left) / 2;
            // mid * mid <= x 会overflow，转换为：
            // mid <= x / mid
            if (mid <= x / mid) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // 此时left == right。取值范围为：[1, x]。
        // 当left == 1的时候，说明搜索区间内没有满足条件的数字。
        // 而1一定在搜索空间内，对于x（大于等于2），一定满足1 * 1 <= x；
        // 所以此时left/right的取值范围为：[2, x]。
        return left - 1;
    }
};
```

