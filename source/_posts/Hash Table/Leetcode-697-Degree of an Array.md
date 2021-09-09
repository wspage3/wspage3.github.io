---
title: Leetcode-697.Degree of an Array
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Hash Table
---

## 一、题意

### 0、题目链接
[697.Degree of an Array](https://leetcode.com/problems/degree-of-an-array/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
nums的degree定义为：nums中元素的最大频率；
求出一个满足条件的最短子数组，输出其长度；
满足条件：该子数组与nums有相同的degree；
【要求】
无；

### 2、Example
Input: nums = [1,2,2,3,1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 50000
2. 0 <= nums[i] <= 49999

## 二、题解

### 1、Solution-1：Straightforward-Hash Table
1. 哈希表m：记录nums中每个数字对应的索引列表；

2. freq：nums中的最大频率，即degree；

3. 首先遍历一次nums：维护哈希表m；求出freq；

4. 然后遍历一次哈希表m：对于满足条件（出现了freq次）的数字，求其最小的范围；

5. Code(https://leetcode.com/submissions/detail/428566761/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, vector<int>> m;
        int freq = 0;
        for (int i = 0; i <= n - 1; i++) {
            m[nums[i]].push_back(i);
            int size = m[nums[i]].size();
            freq = max(freq, size);
        }
        
        int ans = INT_MAX;
        for (auto kv : m) {
            if (kv.second.size() == freq) {
                int l = kv.second.front();
                int r = kv.second.back();
                ans = min(ans, r - l + 1);
            }
        }
        
        return ans;
    }
};
```