---
title: Leetcode-704.Binary Search
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[704.Binary Search](https://leetcode.com/problems/binary-search/)

### 1、Description
【输入】
给定一个有序（升序）的整数数组nums，大小为n；
给定一个整数target；
【输出】
如果target在nums出现的话，输出target的位置；
否则，输出-1；
【要求】
无；

### 2、Example
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 10000
2. -9999 <= nums[i] <= 9999
2. nums中所有元素均只出现一次；

## 二、题解

### 0、思考
1. 二分搜索模板题；

### 1、Solution-1：Binary Search
1. 直接对[0, n - 1]进行二分搜索。二分模板1。

2. Code(https://leetcode.com/submissions/detail/285484689/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        // Solution-1：二分模板1。
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (target > nums[mid]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
};
```

### 2、Solution-2：Binary Search
1. 对[0, n)进行二分搜索，找出第一个x，使得：nums[x] >= target。

2. 然后再判断x的合法性：
如果x == n，说明target比nums中所有元素都要大，返回-1。
如果nums[x] != target，说明target在nums中不存在。

3. Code(https://leetcode.com/submissions/detail/330817868/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        // Solution-2：二分模板2。
        int n = nums.size();
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
        if (left == n) {
            return -1;
        }
        return nums[left] == target ? left : -1;
    }
};
```

### 3、Solution-3：Binary Search
1. 对[0, n)进行二分搜索，找出最后一个x，使得：nums[x] <= target。

2. Code(https://leetcode.com/submissions/detail/330827469/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        // Solution-3：二分模板3。
        int n = nums.size();
        int left = 0;
        int right = n;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // 此时一定有：left == right；其取值范围是：[0, n]。
        if (left == 0) {
            return -1;
        }
        return nums[left - 1] == target ? left - 1 : -1;
    }
};
```