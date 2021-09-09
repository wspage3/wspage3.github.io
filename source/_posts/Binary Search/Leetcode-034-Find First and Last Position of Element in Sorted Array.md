---
title: Leetcode-034.Find First and Last Position of Element in Sorted Array
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[034.Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### 1、Description
【输入】
给定一个有序（升序）的整数数组nums，大小为n；
给定一个整数target；
【输出】
如果target在nums出现的话，输出target第一次出现的位置、最后一次出现的位置；
否则，输出[-1, -1]；
【要求】
时间复杂度为O(log n)；

### 2、Example
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

<!-- more -->

### 3、Corner Case
1. n == 0的时候，不管target多少，返回[-1, -1]即可；
2. 考虑n为正数的情况；
3. 当target > nums[n - 1] || target < nums[0]的时候，返回[-1, -1]即可；

## 二、题解

### 0、思考
1. 二分查找模板题：找左边界、右边界。

### 1、Solution-1：Binary Search
1. 左边界：找出第一个满足条件（大于等于target）的元素。使用二分模板-2即可。

2. 右边界：找出最后一个满足条件（小于等于target）的元素。使用二分模板-3即可。

3. Follow Up：若数组是降序排列的，需要寻找target的左右边界。

4. 左边界：第一个满足条件的元素（小于等于target）；
右边界：最后一个满足条件的元素（大于等于target）；

5. Code(https://leetcode.com/submissions/detail/331203027/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> ans(2, -1);
        int n = nums.size();
        if (n == 0) {
            return ans;
        }
        if (target > nums[n - 1] || target < nums[0]) {
            return ans;
        }
        
        // 1. 寻找target的左边界
        int left = 0;
        int right = n;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        // 此时left == right，可能的取值范围为：[0, n]。
        // 2. 因为已经判断过了target是否大于nums[n - 1]，所以此处left在[0, n - 1]范围内。
        // 3. 判断left位置的数字是否是target。
        if (nums[left] != target) {
            return ans;
        }
        ans[0] = left;
        
        // 4. 寻找target的右边界
        left = 0;
        right = n;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // 此时left == right，可能的取值范围为：[0, n]。
        // 2. 因为已经判断过了target是否小于nums[0]，所以此处left在[1, n]范围内。
        // 3. 因为已经判断过了target存在，所以不必判断位置(left - 1)的数字是否是target。
        ans[1] = left - 1;
        
        return ans;
    }
};
```

