---
title: Leetcode-053.Maximum Subarray
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[053.Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
求出nums的所有非空子数组中，其sum最大的那个，输出这个sum；
【要求】
无；

### 2、Example
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 20000
2. n == 1时，输出nums[0]；
3. INT_MIN <= nums[i] <= INT_MAX

## 二、题解

### 0、思考
1. 对于一个长度为n的数组nums，其非空子数组有n * (n + 1) / 2个；

2. 暴力解法：O(n * n)；

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：以nums[i]开头的所有子数组中，最大的sum；

2. 答案
max(dp[0]-dp[n - 1])；

3. 初始化
dp[n - 1] = nums[n - 1]；

4. 递推式
dp[i] = max(nums[i], nums[i] + dp[i + 1]);

5. Code(https://leetcode.com/submissions/detail/428486083/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n, 0);
        // 1. init
        dp[n - 1] = nums[n - 1];
        int ans = dp[n - 1];
        // 2. solve
        for (int i = n - 2; i >= 0; i--) {
            dp[i] = max(nums[i], nums[i] + dp[i + 1]);
            ans = max(ans, dp[i]);
        }
        // 3. return
        return ans;
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/428492546/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        // 1. init
        int next = nums[n - 1];
        int ans = next;
        // 2. solve
        for (int i = n - 2; i >= 0; i--) {
            int cur = max(nums[i], nums[i] + next);
            ans = max(ans, cur);
            next = cur;
        }
        // 3. return
        return ans;
    }
};
```