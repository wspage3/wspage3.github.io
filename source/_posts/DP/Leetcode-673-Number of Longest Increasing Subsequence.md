---
title: Leetcode-673.Number of Longest Increasing Subsequence
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[673.Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
求出nums的最长上升子序列的个数；
上升子序列：后一个元素严格大于前一个元素；
【要求】
无；

### 2、Example
Input: nums = [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 2000
2. n == 1时，输出1；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：以nums[i]结尾的所有上升子序列中，最大的长度；
cnt[i]表示：以nums[i]结尾的最长上升子序列，其方案数；

2. 答案
sum(cnt[i])；
其中i满足：dp[i] == maxLen；

3. 初始化
dp[0] = 1；
cnt[0] = 1；

4. 递推式
* nums[i]连接到nums[j]后面，构成的子序列，刷新了已有的长度记录
* nums[i]连接到nums[j]后面，构成的子序列，达到了已有的长度记录

5. Code(https://leetcode.com/submissions/detail/430101463/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 1;
        }
        
        // dp[i]表示：以nums[i]结尾的所有上升子序列中，最大的长度；
        vector<int> dp(n);
        dp[0] = 1;
        // cnt[i]表示：以nums[i]结尾的最长上升子序列，其方案数；
        vector<int> cnt(n);
        cnt[0] = 1;
        
        int maxLen = 1;
        for (int i = 1; i <= n - 1; i++) {
            dp[i] = 1;
            cnt[i] = 1;
            for (int j = 0; j <= i - 1; j++) {
                if (nums[i] > nums[j]) {
                    if (dp[j] + 1 > dp[i]) {
                        // nums[i]连接到nums[j]后面，是最长的
                        cnt[i] = cnt[j];
                        dp[i] = dp[j] + 1;
                    } else if (dp[j] + 1 == dp[i]) {
                        // 此时nums[i]连接到nums[0, j - 1]某个数字后面，是最长的
                        // 同时nums[i]连接到nums[j]后面，也是最长的
                        cnt[i] += cnt[j];
                    }
                }
            }
            maxLen = max(maxLen, dp[i]);
        }
        
        int ans = 0;
        for (int i = 0; i <= n - 1; i++) {
            if (dp[i] == maxLen) {
                ans += cnt[i];
            }
        }
        return ans;
    }
};
```