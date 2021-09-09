---
title: Leetcode309-Best Time to Buy and Sell Stock with Cooldown
---

## 一、题意
题目链接: [Leetcode309-Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
### 1.Description
给定一个长度为n的整数数组prices；prices[i]代表第i天股票的价格。
最多交易次数不限，求出你最多能赚多少钱。
前提是：买入下一股的时候，必须将上一股卖掉。
而且建议冷却期为1天，即：今天卖掉，后天才能买入。
### 2.Example
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
### 3.Corner Case
n: 当n为0/1的时候，直接返回0；
赚不到钱则返回0；

## 二、题解
### Solution-1：Dynamic Programming
1.对于任意一天i，有三种选择：buy, sell, rest
2.状态转移方程：
buy[i] = max(buy[i - 1], sell[i - 2] - prices[i]);//左边代表：今天休息；右边代表：今天用前天的利润来进一批货。
sell[i] = max(sell[i - 1], buy[i - 1] + prices[i]);//左边代表：今天休息；右边代表：今天在昨天进货的基础上，卖掉货。
buy[i]:时间范围[0, i]内，最后一次操作是buy或者rest，所能获得的最高利润
sell[i]:时间范围[0, i]内，最后一次操作是sell或者rest，所能获得的最高利润

时间复杂度：O(n)
空间复杂度：O(n)

3.Code(https://leetcode.com/submissions/detail/284970939/)

### Solution-2：Dynamic Programming + Optimization
1.优化空间复杂度

时间复杂度：O(n)
空间复杂度：O(1)

3.Code(https://leetcode.com/submissions/detail/284973472/)









