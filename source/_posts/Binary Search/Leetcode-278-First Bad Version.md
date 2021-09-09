---
title: Leetcode-278.First Bad Version
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[278.First Bad Version](https://leetcode.com/problems/first-bad-version/)

### 1、Description
【背景】
发现当前的产品版本中，有bug。
由于每个版本都是基于上一个版本，迭代开发的。所以说当某个版本引入bug后，后面所有的版本都有bug。
现在希望找出来是哪个版本引入了bug。
有一个API-isBadVersion可以根据版本号来确定当前版本是否有bug。如果有bug，则返回true。否则，返回false。
【输入】
给定一个整数n，代表有n个版本号。
版本号从1开始，到n结束。
【输出】
输出第一个出现bug的那个版本号。
【要求】
使得调用API-isBadVersion的次数尽可能的少。

### 2、Example
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 

<!-- more -->

### 3、Corner Case
1. n <= 0的时候，没有任何版本，不考虑这种情况；
2. 考虑n为正数的情况；

## 二、题解

### 0、思考
1. 问题转化为：在区间[1, n]中查找，第一个满足条件的数字x。条件：isBadVersion(x) == true。

### 1、Solution-1：Binary Search
1. 找出第一个满足条件的元素。使用二分模板-2即可。

2. Code(https://leetcode.com/submissions/detail/285606627/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int firstBadVersion(int n) {
        if (n < 1) {
            return 0;
        }
        // 二分模板2-找出第一个满足条件的元素。
        int left = 1;
        int right = n;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (isBadVersion(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

