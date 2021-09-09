---
title: Leetcode-540.Single Element in a Sorted Array
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Search
---

## 一、题意

### 0、题目链接
[540.Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/)

### 1、Description
【输入】
给定一个大小为n的整数数组nums；nums非空；
其中除了x出现一次外，其它元素都出现了两次；
且nums是有序的；
【输出】
找出x；
【要求】
时间复杂度：O(log n)
空间复杂度：O(1)

### 2、Example
Input: nums = [3,3,7,7,10,11,11]
Output: 10

<!-- more -->

### 3、Corner Case
1. n > 0；
2. n为奇数；
3. n == 1时，返回nums[0]；

## 二、题解

### 0、思考
1. 本题类似于：136.Single Number。都是在nums中寻找只出现一次的数字x；

2. 在本题中，nums的性质更特殊：是有序排列的。

3. 可以采用136中的解法：哈希表方法、异或方法。但时间复杂度为O(n)；

### 1、Solution-1：Binary Search
1. 回忆二分搜索的核心：通过一次比较，可以把搜索空间从n降低到n / 2；

2. 由于n一定是奇数，所以取mid后，左右两边长度相同；有以下两种情况：
情况1：mid是奇数：即左右两边的元素个数都是奇数。例如：[1122344]；
情况2：mid是偶数：即左右两边的元素个数都是偶数。例如：[112234455]；

3. 分析情况1：
若此时nums[mid] == nums[mid - 1]，即[left, mid]范围内有偶数个元素，且最后两个元素是一对。说明：x一定出现在[mid + 1, right]中。
若此时nums[mid] != nums[mid - 1]，即[left, mid]范围内有偶数个元素，且最后两个元素不是一对。说明：x一定出现在[left, mid - 1]中。

4. 分析情况2：
若此时nums[mid] == nums[mid - 1]，即[left, mid]范围内有奇数个元素，且最后两个元素是一对。说明：x一定出现在[left, mid - 2]中。
若此时nums[mid] != nums[mid - 1]，即[left, mid]范围内有奇数个元素，且最后两个元素不是一对。说明：x一定出现在[mid, right]中。

5. 边界值分析：每次的搜索空间一定是奇数。
当搜索空间为7时：搜索空间降低为3。
当搜索空间为5时：搜索空间降低为1或者3。
当搜索空间为3时：搜索空间降低为1。
当搜索空间为1时：left == right，返回nums[left]即可。

6. Code(https://leetcode.com/submissions/detail/347480473/)
AC
时间复杂度：O(log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (mid % 2 == 1) {
                if (nums[mid] == nums[mid - 1]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } else {
                if (nums[mid] == nums[mid - 1]) {
                    right = mid - 2;
                } else {
                    left = mid;
                }
            }
        }
        return nums[left];
    }
};
```