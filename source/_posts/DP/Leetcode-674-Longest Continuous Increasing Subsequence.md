---
title: Leetcode-674.Longest Continuous Increasing Subsequence
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[674.Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
求出nums的所有上升子数组中，最长的那个，输出这个长度；
【要求】
无；

### 2、Example
Input: nums = [1,3,5,4,7]
Output: 3
Explanation: The longest continuous increasing subsequence is [1,3,5] with length 3.

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 10000
2. n == 0时，输出0；
3. n == 1时，输出1；

## 二、题解

### 0、思考
1. 对于一个长度为n的数组来说：
【子数组】（非空）的个数为：n * (n + 1) / 2；复杂度：O(n * n)
【子序列】（非空）的个数为：(2 ^ n) - 1；复杂度：O(2 ^ n)

2. Leetcode-300-Longest Increasing Subsequence
求nums的最长上升子序列；

3. 本题求nums的最长上升子数组；

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：以nums[i]结尾的上升子数组中，最大的长度；

2. 答案
max(dp[0]-dp[n - 1])；

3. 初始化
dp[0] = 1；

4. 递推式
* nums[i]不能接在nums[i - 1]的后面：1
* nums[i]可以接在nums[i - 1]的后面：1 + dp[i - 1]

5. Code(https://leetcode.com/submissions/detail/430105858/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return n;
        }
        
        vector<int> dp(n);
        dp[0] = 1;
        int maxLen = 1;
        
        for (int i = 1; i <= n - 1; i++) {
            if (nums[i] > nums[i - 1]) {
                dp[i] = 1 + dp[i - 1];
            } else {
                dp[i] = 1;
            }
            maxLen = max(maxLen, dp[i]);
        }
        
        return maxLen;
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/430106147/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return n;
        }
        
        int pre = 1;
        int maxLen = 1;
        
        for (int i = 1; i <= n - 1; i++) {
            int cur;
            if (nums[i] > nums[i - 1]) {
                cur = 1 + pre;
            } else {
                cur = 1;
            }
            maxLen = max(maxLen, cur);
            pre = cur;
        }
        
        return maxLen;
    }
};
```