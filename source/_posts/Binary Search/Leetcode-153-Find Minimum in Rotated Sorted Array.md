---
title: Leetcode-153.Find Minimum in Rotated Sorted Array
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[153.Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；性质如下：
* 无重复数字；
* 是一个升序数组，经过rotate转化得到的；
* rotate：将数组最后一个数字，放到数组的开头；

【输出】
找出nums中的最小值，并返回；
【要求】
无；

### 2、Example
Input: [3,4,5,1,2] 
Output: 1

<!-- more -->

### 3、Corner Case
1. n == 0时：无意义，返回错误码；
2. n == 1时：返回nums[0]即可；
3. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. 由于数组无重复数字，所以rotate之前是一个严格升序的数组；

2. rotate后，有两种情况：
仍是一个严格升序数组；
前后两部分都是严格升序的；且前部分的最小数 > 后部分的最大数；

3. 回顾常规的二分搜索：核心在于数组是有序的，我们可以根据nums[mid]和target的大小关系，每次舍弃一半搜索空间。

### 1、Solution-1：Binary Search
1. 思路：二分搜索，每次舍弃一半的搜索空间。

2. 考虑在区间[left, right]上（元素个数 >= 2），寻找最小值：

3. 预判断：
若nums[left] < nums[right]：说明[left, right]是有序的，返回nums[left]即可；
若nums[left] == nums[right]：不可能。因为无重复元素，且区间内元素个数 >= 2，即left != right；
若nums[left] > nums[right]：说明nums可以分成前后两部分，每部分都是严格升序的。这种情况下，对其进行二分；

4. 二分过程1：找mid；
在区间[left, right]上（元素个数 >= 2），计算mid = left + (right - left) / 2;
当区间元素个数 == 2时：mid = left = right - 1;
当区间元素个数 > 2时：left < mid < right;

5. 二分过程2：判断mid；
若nums[mid] > nums[right]，则说明mid此时在左侧。而最小值一定在右侧[mid + 1, right]；
若nums[mid] == nums[right]，不可能。因为无重复元素，且mid不可能等于right；
若nums[mid] < nums[right]，则说明mid此时在右侧。而最小值一定在包含其的左侧，即：[left, mid]。

6. 二分过程3：边界值检查；
若搜索区间大小为6：mid将大小为6的搜索区间分成：3、3；左、右继续进入二分循环；
若搜索区间大小为5：mid将大小为5的搜索区间分成：3、2；左、右继续进入二分循环；
若搜索区间大小为4：mid将大小为4的搜索区间分成：2、2；左、右继续进入二分循环；
若搜索区间大小为3：mid将大小为3的搜索区间分成：2、1；左继续进入二分循环；右找到答案了（left == right）；
若搜索区间大小为2：mid将大小为3的搜索区间分成：1、1；左找到答案了；右找到答案了；
若搜索区间大小为1：不会进入二分循环，此时left == right，返回nums[left]就是最小值；

7. Code(https://leetcode.com/submissions/detail/335669649/)
AC
时间复杂度：O(log n)
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
        // 需要保证[left, right]至少包含两个元素；
        while (left < right) {
            // 1. [left, right]是严格升序的；
            if (nums[left] < nums[right]) {
                return nums[left];
            }
            // 2. [left, right]前后两部分是严格升序的；
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // 此时一定有：left == right。而且就是最小值的索引。
        return nums[left];
    }
};
```

