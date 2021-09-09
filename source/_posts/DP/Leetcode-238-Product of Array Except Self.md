---
title: Leetcode-238.Product of Array Except Self
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[238.Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
求出一个数组ans；
其中ans[i]：nums中除了nums[i]之外，其余元素的乘积；
【要求】
不能使用除法；
O(n)的时间复杂度；
O(1)的空间复杂度；

### 2、Example
Input:  [1,2,3,4]
Output: [24,12,8,6]

<!-- more -->

### 3、Corner Case
1. n >= 2
2. 题目保证nums的任意前缀、nums的任意后缀、nums本身，其product在int32范围内；

## 二、题解

### 0、思考
1. 本质上求：nums[i]右边所有元素的乘积、nums[i]左边所有元素的乘积；

### 1、Solution-1：Dynamic Programming
1. dpRight[i]表示：nums[i]右边所有元素的乘积；
dpLeft[i]表示：nums[i]左边所有元素的乘积

2. 答案
ans[i] = dpLeft[i] * dpRight[i]；

3. 初始化
dpRight[n - 1] = 1;
dpLeft[0] = 1;

4. 递推式
* dpRight[i] = nums[i + 1] * dpRight[i + 1];
* dpLeft[i] = nums[i - 1] * dpLeft[i - 1];

5. Code(https://leetcode.com/submissions/detail/428829156/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        
        // 1. dpRight[i]：nums[i]右边所有元素的乘积
        vector<int> dpRight(n);
        dpRight[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            dpRight[i] = nums[i + 1] * dpRight[i + 1];
        }
        
        // 2. dpLeft[i]：nums[i]左边所有元素的乘积
        vector<int> dpLeft(n);
        dpLeft[0] = 1;
        for (int i = 1; i <= n - 1; i++) {
            dpLeft[i] = nums[i - 1] * dpLeft[i - 1];
        }
        
        // 3. solve
        vector<int> ans(n);
        for (int i = 0; i <= n - 1; i++) {
            ans[i] = dpLeft[i] * dpRight[i];
        }
        return ans;
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/428833536/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        
        // 1. dpRight[i]：nums[i]右边所有元素的乘积
        int next = 1;
        ans[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            int cur = nums[i + 1] * next;
            ans[i] = cur;
            next = cur;
        }
        
        // 2. dpLeft[i]：nums[i]左边所有元素的乘积
        int pre = 1;
        ans[0] *= 1;
        for (int i = 1; i <= n - 1; i++) {
            int cur = nums[i - 1] * pre;
            ans[i] *= cur;
            pre = cur;
        }
        
        return ans;
    }
};
```