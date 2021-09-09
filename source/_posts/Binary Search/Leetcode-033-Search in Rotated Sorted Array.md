---
title: Leetcode-033.Search in Rotated Sorted Array
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[033.Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

### 1、Description
【输入】
给定一个整数target；
给定一个长度为n的整数数组nums；性质如下：
* 无重复数字；
* 是一个升序数组，经过rotate转化得到的；
* rotate：将数组最后一个数字，放到数组的开头；

【输出】
判断target是否存在；若存在返回其index，否则返回-1；
【要求】
时间复杂度为O(log n)；

### 2、Example
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

<!-- more -->

### 3、Corner Case
1. n == 0时：返回-1；
2. 考虑n >= 1的情况；

## 二、题解

### 0、思考
1. 在153.Find Minimum in Rotated Sorted Array中，nums的特点和本题的特点完全一样。我们在O(log n)的时间内找到了nums中的最小值；

### 1、Solution-1：Binary Search
1. 由于nums只有两种可能：
[0, n - 1]完全是升序的，使用常规的二分搜索即可（二分模板1）；
[0, k - 1]、[k, n - 1]两个区间是升序的，且：前一区间内的最小值 > 后一区间的最大值；

2. 观察：nums中的最小值，一定在位置k处。

3. 首先，找到k的位置。

4. 然后，判断target：在右区间[nums[k], nums[n - 1]]还是在左区间[nums[0], nums[k - 1]]中；

5. 最后，在完全递增的区间内，进行二分搜索即可（二分模板1）。

7. Code(https://leetcode.com/submissions/detail/335682569/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
private:
    int searchPos(vector<int>& nums) {
        // 参考：153. Find Minimum in Rotated Sorted Array
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        while (left < right) {
            if (nums[left] < nums[right]) {
                return left;
            }
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0) {
            return -1;
        }
        
        // Step-1: 找出nums中第一个小于等于nums[n - 1]的位置k。
        // 这样的话，nums就分成[0, k - 1]和[k, n - 1]两个有序子数组了；
        int k = searchPos(nums);
        
        // Step-2: 选择搜索区间：左/右
        int left;
        int right;
        if (target >= nums[k] && target <= nums[n - 1]) {
            left = k;
            right = n - 1;
        } else {
            left = 0;
            right = k - 1;
        }
        
        // Step-3: 在已选的搜索区间内，查找target是否存在。（二分模板1）
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
};
```

