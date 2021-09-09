---
title: Leetcode-322.Coin Change
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[322.Coin Change](https://leetcode.com/problems/coin-change/)

### 1、Description
【输入】
给定一个整数target；
给定一个长度为n的数组coins，代表你有这些面额的硬币；
【输出】
求最少使用多少个硬币，使得其sum为target；
每种硬币有任意多个；
【要求】
无；

### 2、Example
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 12
2. 1 <= coins[i] <= INT_MAX
3. 0 <= target <= 10000
4. 当target为0时，输出0；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：最少使用多少个硬币，使得其sum为i；

2. 答案：dp[target]；

3. 初始化
dp[0] = 0；（当i在coins中出现的时候，此时dp[i] = 1 + dp[0] = 1）

4. 递推式
dp[i] = min(dp[i], 1 + dp[i - coin]);

5. Code(https://leetcode.com/submissions/detail/429149611/)
AC
时间复杂度：O(target * n)
空间复杂度：O(target)
```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int target) {
        if (target == 0) {
            return 0;
        }
        
        vector<int> dp(target + 1);
        dp[0] = 0;
        for (int i = 1; i <= target; i++) {
            int minCnt = INT_MAX;
            for (int coin : coins) {
                if (coin > i || dp[i - coin] == INT_MAX) {
                    continue;
                }
                minCnt = min(minCnt, 1 + dp[i - coin]);
            }
            dp[i] = minCnt;
        }
        return dp[target] == INT_MAX ? -1 : dp[target];
    }
};
```