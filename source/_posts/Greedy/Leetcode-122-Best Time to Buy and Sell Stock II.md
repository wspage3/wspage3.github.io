---
title: Leetcode-122.Best Time to Buy and Sell Stock II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Greedy
---

## 一、题意

### 0、题目链接
[122.Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
nums[i]代表：第i天的股价；
【输出】
小王买卖股票不受次数限制；
但必须先买入，再卖出；买之前必须卖出已有的；
求出小王最多能赚多少钱；
【要求】
无；

### 2、Example
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

<!-- more -->

### 3、Corner Case
1. 答案至少为0，即不赔不赚；
2. n <= 1时，输出0；

## 二、题解

### 0、思考
1. 贪心思路：只要明天比今天贵，就今天买明天卖；

### 1、Solution-1：Greedy
1. Code(https://leetcode.com/submissions/detail/428850356/)
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
        for (int i = 1; i <= n - 1; i++) {
            int diff = nums[i] > nums[i - 1] ? nums[i] - nums[i - 1] : 0;
            ans += diff;
        }
        return ans;
    }
};
```