---
title: Leetcode-121.Best Time to Buy and Sell Stock
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[121.Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
nums[i]代表：第i天的股价；
【输出】
小王最多能买卖一次股票；
且必须先买入，再卖出；
求出小王最多能赚多少钱；
【要求】
无；

### 2、Example
Input: [7,1,5,3,6,4]
Output: 5

<!-- more -->

### 3、Corner Case
1. 答案至少为0，即不赔不赚；
2. n <= 1时，输出0；

## 二、题解

### 0、思考
1. 实际上要找一个nums的子数组，数组的左端代表买入，右端代表卖出；

2. 对于一个长度为n的数组nums，其非空子数组有n * (n + 1) / 2个；

3. 暴力解法：O(n * n)；

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：nums[i + 1, n - 1]中，最大的数是多少；

2. 答案
max(dp[i]-nums[i])；

3. 初始化
dp[n - 1] = 0；

4. 递推式
dp[i] = max(nums[i + 1], dp[i + 1]);

5. Code(https://leetcode.com/submissions/detail/428577925/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int maxProfit(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return 0;
        }
        
        int ans = 0;
        vector<int> dp(n);
        // 1. init
        dp[n - 1] = 0;
        // 2. solve
        for (int i = n - 2; i >= 0; i--) {
            dp[i] = max(nums[i + 1], dp[i + 1]);
            ans = max(ans, dp[i] - nums[i]);
        }
        // 3. return
        return ans;
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/428578569/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int maxProfit(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return 0;
        }
        
        int ans = 0;
        // 1. init
        int next = 0;
        // 2. solve
        for (int i = n - 2; i >= 0; i--) {
            int cur = max(nums[i + 1], next);
            ans = max(ans, cur - nums[i]);
            next = cur;
        }
        // 3. return
        return ans;
    }
};
```