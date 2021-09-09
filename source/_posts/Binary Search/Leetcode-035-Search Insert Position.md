---
title: Leetcode-035.Search Insert Position
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[035.Search Insert Position](https://leetcode.com/problems/search-insert-position/)

### 1、Description
【输入】
给定一个有序的整数数组nums，大小为n；
给定一个整数target；
【输出】
如果target在nums出现的话，输出target的位置；
否则，输出target应该插入到哪个位置，使得nums仍然有序；
【要求】
无；

### 2、Example
Input: [1,3,5,6], 2
Output: 1

Input: [1,3,5,6], 7
Output: 4

<!-- more -->

### 3、Corner Case
1. n == 0的时候，不管target多少，返回0即可；
2. 考虑n为正数的情况；
2. nums中所有元素均只出现一次；

## 二、题解

### 0、思考
1. 类似于278.First Bad Version，都是搜索满足某个条件的第一个元素x。
2. 278中的条件：使得isBadVersion(x) == true。
3. 本题中的条件：使得nums[x] >= target。

### 1、Solution-1：Binary Search
1. 找出第一个满足条件的元素。使用二分模板-2即可。

2. Code(https://leetcode.com/submissions/detail/330803751/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int l = 0;
        int r = n;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        // 此时一定有：l == r；其取值范围是：[0, n]。
        return l;
    }
};
```

