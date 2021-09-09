---
title: Leetcode-081.Search in Rotated Sorted Array II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[081.Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

### 1、Description
【输入】
给定一个整数target；
给定一个长度为n的整数数组nums；性质如下：
* 可能有重复数字；
* 是一个升序数组，经过rotate转化得到的；
* rotate：将数组最后一个数字，放到数组的开头；

【输出】
判断target是否存在；若存在返回true，否则返回false；
【要求】
分析若nums中有重复数字，是否影响时间复杂度，为什么？

### 2、Example
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true

<!-- more -->

### 3、Corner Case
1. n == 0时：返回false；
2. n == 1时：返回nums[0] == target；
3. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. 在033.Search in Rotated Sorted Array中，nums中无重复元素。我们在O(log n)的时间内判断target是否存在；

2. 在154.Find Minimum in Rotated Sorted Array II中，nums的特点和本题的特点完全一样。我们在平均O(log n)、最坏O(n)的时间内找到了nums中的最小值；

### 1、Solution-1：Binary Search
1. 当nums中有重复数字时，可以分成以下几种场景：

2. 情况1：nums整体仍有序，且nums[left] == nums[right]。例如：[2, 2, 2, 2, 2]
若target == 2，则第一次就找到了，返回true。
若target != 2，则：
2 == 2，right--，在[2, 2, 2, 2]中搜索。
2 == 2，right--，在[2, 2, 2]中搜索。
2 == 2，right--，在[2, 2]中搜索。
2 == 2，right--，在[2]中搜索。
2 == 2，right--，在[]中搜索。没找到，return false。

3. 情况2：nums整体仍有序，且nums[left] < nums[right]。例如：[2, 2, 2, 3, 3]
直接走常规二分搜索流程（二分模板1）；

4. 情况3：nums整体无序，分为两个有序部分，且左部分最小值 > 右部分最大值。例如：[3, 3, 4, 4, 5, 1, 1, 2, 2]
若target存在，如4：
5 > 2，mid在左边有序部分。且 target-4 >= left-3 && target-4 < mid-5，则在mid左边，[3, 3, 4, 4]中搜索。
3 < 4，mid在右边有序部分。且 target-4 > mid-3 && target-4 <= right-4，则在mid右边，[4, 4]中搜索。
4 == 4，找到，return true。

5. 情况4：nums整体无序，分为两个有序部分，且左部分最小值 = 右部分最大值。例如：[3, 4, 4, 5, 1, 1, 2, 2, 3]
若target存在，如2：
1 < 3，mid在右边有序部分。且target-2 > mid-1 && target-2 <= right-3，则在mid右边，[1, 2, 2, 3]中搜索。
2 == 2，找到，return true。

6. Code(https://leetcode.com/submissions/detail/336111967/)
AC
时间复杂度：O(n) // 在最坏情况下，时间复杂度为O(n)；平均情况下，时间复杂度为O(log n)，还是优于O(n)的暴力查找的；
空间复杂度：O(1)
```C++
class Solution {
private:
    bool binarySearch1(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return true;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return false;
    }
public:
    bool search(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0) {
            return false;
        }
        if (n == 1) {
            return (nums[0] == target);
        }
        
        // 若nums[0] < nums[n - 1]，一定是情况2。直接进行常规的二分搜索（二分模板1）；
        if (nums[0] < nums[n - 1]) {
            return binarySearch1(nums, target);
        }
        
        // 当情况1、3、4的时候，进入下面的二分；
        int left = 0;
        int right = n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return true;
            } else if (nums[mid] > nums[right]) {
                // 此时，nums[mid]一定在左边部分。所以，[left, mid]一定是有序的。
                if (target >= nums[left] && target < nums[mid]) {
                    // target若等于nums[mid]，在上面就return了。
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
                
            } else if (nums[mid] < nums[right]) {
                // 此时，nums[mid]一定在右边部分。所以，[mid, right]一定是有序的。
                if (target > nums[mid] && target <= nums[right]) {
                    // target若等于nums[mid]，在上面就return了。
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } else {
                // 若nums[mid] == nums[right]，则像154.Find Minimum in Rotated Sorted Array II一样，只能线性的缩小搜索空间。
                right--;
            }
        }
        return false;
    }
};
```
