---
title: Leetcode714-Best Time to Buy and Sell Stock with Transaction Fee
---

## 一、题意
题目链接: [Leetcode714-Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)
### 1.Description
给定一个长度为n的整数数组prices；prices[i]代表第i天股票的价格。
非负数fee代表每次交易需要的费用。
最多交易次数不限，求出你最多能赚多少钱。
前提是：买入下一股的时候，必须将上一股卖掉。
### 2.Example
Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
Buying at prices[0] = 1
Selling at prices[3] = 8
Buying at prices[4] = 4
Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
### 3.Corner Case
0 < prices.length <= 50000.
0 < prices[i] < 50000.
0 <= fee < 50000.

n: 当n为0/1的时候，直接返回0；
赚不到钱则返回0；

## 二、题解
### Solution-1：Dynamic Programming
1.根据leetcode123、188，求出状态转移方程。
由于不考虑交易次数的限制，而加上了考虑fee，所以可以得出以下方程：
dp[i] = max(dp[i - 1], dp[j] + prices[i] - prices[j] - fee);
      = max(dp[i - 1], prices[i] - fee - (prices[j] - dp[j]));
dp[i]含义为：范围[0, i]内，所能获取的最高利润
i:[1, n - 1]  dp[0] = 0
j:[0, i - 1]  当j为i的时候，相当于今天买了，今天卖，肯定亏一次手续费。

2.所以i在[1, n - 1]内循环，对于每个固定的i，在[0, i - 1]中找出一个j，使得表达式"(prices[j] - dp[j])"的值最小。
找出来j的位置后，dp[i]就可以求出；结果返回dp[n - 1]即可。

时间复杂度：O(n ^ 2)
空间复杂度：O(n)

结果和预期一样，TLE

3.Code(https://leetcode.com/submissions/detail/284928102/)

### Solution-2：Dynamic Programming + Optimization
1.首先，由于i每次递增1：
对于当前的i来说，首要任务是在[0, i - 1]找出目标表达式"(prices[j] - dp[j])"的最小值。
而前一轮的i - 1，已经计算出了[0, i - 2]内目标表达式的最小值。
所以对于当前i来说，直接复用该结果，可以使得O(n)的time complexity降低到O(1)。

时间复杂度：O(n)
空间复杂度：O(n)

2.Code(https://leetcode.com/submissions/detail/284929795/)

3.由于dp[i]只需要知道dp[i - 1]的值就可以进行计算了。所以可以使得O(n)的space complexity降低到O(1)。

时间复杂度：O(n)
空间复杂度：O(1)

4.Code(https://leetcode.com/submissions/detail/284930112/)
