---
title: Leetcode-154.Find Minimum in Rotated Sorted Array II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[154.Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；性质如下：
* 可能有重复数字；
* 是一个升序数组，经过rotate转化得到的；
* rotate：将数组最后一个数字，放到数组的开头；

【输出】
找出nums中的最小值，并返回；
【要求】
分析若nums中有重复数字，是否影响时间复杂度，为什么？

### 2、Example
Input: [2,2,2,0,1]
Output: 0

<!-- more -->

### 3、Corner Case
1. n == 0时：无意义，返回错误码；
2. n == 1时：返回nums[0]即可；
3. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. 是153.Find Minimum in Rotated Sorted Array的升级版；

2. 唯一区别在于：本题允许nums中有重复数字；

### 1、Solution-1：Binary Search
1. 当nums中有重复数字时，可以分成以下几种场景：

2. nums整体仍有序，且nums[left] == nums[right]。例如：[2, 2, 2, 2, 2]
2 = 2，right向前移动一位，在[2, 2, 2, 2]中搜索；
2 = 2，right向前移动一位，在[2, 2, 2]中搜索；
2 = 2，right向前移动一位，在[2, 2]中搜索；
2 = 2，right向前移动一位，在[2]中搜索；跳出循环，返回2；

3. nums整体仍有序，且nums[left] < nums[right]。例如：[2, 2, 2, 3, 3]
因为nums[left] < nums[right]，直接返回nums[left]即可；

4. nums整体无序，分为两个有序部分，且左部分最小值 > 右部分最大值。例如：[3, 3, 4, 4, 5, 1, 1, 2, 2]
5 > 2，在[1, 1, 2, 2]中搜索；
1 < 2，在[1, 1]中搜索；
1 = 1，right向前移动一位，在[1]中搜索；跳出循环，返回1；

5. nums整体无序，分为两个有序部分，且左部分最小值 = 右部分最大值。例如：[3, 4, 4, 5, 1, 1, 2, 2, 3]
1 < 3，在[3, 4, 4, 5, 1]中搜索；
4 > 1，在[5, 1]中搜索；
5 > 1，在[1]中搜索；跳出循环，返回1；

6. Code(https://leetcode.com/submissions/detail/335792404/)
AC
时间复杂度：O(n) // 在最坏情况下，时间复杂度为O(n)；平均情况下，时间复杂度为O(log n)，还是优于O(n)的暴力查找的；
空间复杂度：O(1)
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return -1; // invalid testcase
        }
        if (n == 1) {
            return nums[0];
        }
        
        int left = 0;
        int right = n - 1;
        while (left < right) {
            if (nums[left] < nums[right]) {
                return nums[left];
            }
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else if (nums[mid] < nums[right]) {
                right = mid;
            } else {
                right--;
            }
        }
        return nums[left];
    }
};
```

