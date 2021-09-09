---
title: Leetcode-136.Single Number
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Bit Manipulation
- Hash Table
---

## 一、题意

### 0、题目链接
[136.Single Number](https://leetcode.com/problems/single-number/)

### 1、Description
【输入】
给定一个大小为n的整数数组nums；nums非空；
其中除了x出现一次外，其它元素都出现了两次；
【输出】
找出x；
【要求】
无；

### 2、Example
Input: [4,1,2,1,2]
Output: 4

<!-- more -->

### 3、Corner Case
1. n > 0；
2. n为奇数；
3. n == 1时，返回nums[0]；

## 二、题解

### 1、Solution-1：Hash Table
1. 遍历nums，使用哈希表统计每个数字出现的次数；

2. 找到次数为1的那个数字，即x；

3. Code(https://leetcode.com/submissions/detail/347427032/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int, int> m;
        for (int x : nums) {
            m[x]++;
        }
        for (auto kv : m) {
            if (kv.second == 1) {
                return kv.first;
            }
        }
        return -1; // cannot execute
    }
};
```

### 2、Solution-2：Bit Manipulation
1. 异或的性质：
x ^ x = 0；
x ^ 0 = x；

2. Code(https://leetcode.com/submissions/detail/318443594/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int num : nums) {
            ans = ans ^ num;
        }
        return ans;
    }
};
```