---
title: Leetcode-198.House Robber
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[198.House Robber](https://leetcode.com/problems/house-robber/)

### 1、Description
【输入】
给定一个长度为n的数组nums，代表着n户人家的财产；
【输出】
小王今晚打算去打劫这n户人家；
已知：若相邻的两户人家同时被打劫，则会自动触发警报；
求小王今晚最多能打劫多少财产，输出这个值；
【要求】
无；

### 2、Example
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: 2 + 9 + 1 = 12.

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 100
2. n == 0时，输出0；
3. n == 1时，输出nums[0]；
4. 0 <= nums[i] <= 400

## 二、题解

### 0、思考
1. 错误想法：
计算x = nums[0], nums[2]...
计算y = nums[1], nums[3]...
然后将max(x, y)作为答案；

2. 对于nums = [2, 1, 1, 2]来说，上述方法得到错误的答案；

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：在抢劫了[0, i]这些人家后，最多抢劫到的财产；

2. 答案：dp[n - 1]；

3. 递推式
dp[i] = max(dp[i - 1], nums[i] + dp[i - 2]);

4. Code(https://leetcode.com/submissions/detail/428179734/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        if (n == 1) {
            return nums[0];
        }
        
        vector<int> dp(n, 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i <= n - 1; i++) {
            dp[i] = max(dp[i - 1], nums[i] + dp[i - 2]);
        }
        return dp[n - 1];
    }
};
```

5. 空间优化
计算dp[i]只需要dp[i - 1]和dp[i - 2]；

6. Code(https://leetcode.com/submissions/detail/428180144/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        if (n == 1) {
            return nums[0];
        }
        
        int prepre = nums[0];
        int pre = max(nums[0], nums[1]);
        for (int i = 2; i <= n - 1; i++) {
            int cur = max(pre, nums[i] + prepre);
            prepre = pre;
            pre = cur;
        }
        return pre;
    }
};
```