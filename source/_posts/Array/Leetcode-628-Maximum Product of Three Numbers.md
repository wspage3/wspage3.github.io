---
title: Leetcode-628. Maximum Product of Three Numbers
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Math
---

## 一、题意

### 0、题目链接
[628. Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
从nums中取三个数，使得这三个数的product最大，输出这个product；
【要求】
无；

### 2、Example
Input: nums = [1,2,3,4]
Output: 24

<!-- more -->

### 3、Corner Case
1. n合法：3 <= n <= 10000
2. -1000 <= nums[i] <= 1000

## 二、题解

### 1、Solution-1：Straightforward
1. Leetcode-152.Maximum Product Subarray中：
要使两个数a、b的乘积最大，可以分为三种情况：
* b = 0：a任意选择；
* b > 0：a选择最大的；
* b < 0：a选择最小的；

3. 本题中，也可以分为以下几种情况：
* 3个正数：
[-1, 0, 1, 2, 3, 4]
选择三个最大的；
* 2个正数：
[-4, -3, -2, 5, 6]
[-4, -3, 5, 6]
[-4, 0, 5, 6]
选择一个最大的、两个最小的；
或选择三个最大的；
* 1个正数：
[-4, -3, -2, -1, 5]
[-4, -3, -2, 0, 5]
选择一个最大的、两个最小的；
* 0个正数：
[-4, -3, -2, -1, 0]
[-4, -3, -2, -1, -1]
选择三个最大的；

4. Code(https://leetcode.com/submissions/detail/429076537/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        int n = nums.size();
        int max1 = INT_MIN;
        int max2 = INT_MIN;
        int max3 = INT_MIN;
        int min1 = INT_MAX;
        int min2 = INT_MAX;
        for (int x : nums) {
            if (x > max1) {
                max3 = max2;
                max2 = max1;
                max1 = x;
            } else if (x > max2) {
                max3 = max2;
                max2 = x;
            } else if (x > max3) {
                max3 = x;
            }
            
            if (x < min1) {
                min2 = min1;
                min1 = x;
            } else if (x < min2) {
                min2 = x;
            }
        }
        return max(max1 * max2 * max3, max1 * min1 * min2);
    }
};
```