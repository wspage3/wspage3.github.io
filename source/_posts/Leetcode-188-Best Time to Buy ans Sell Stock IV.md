---
title: Leetcode188-Best Time to Buy and Sell Stock IV
---

## 一、题意
题目链接: [Leetcode188-Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)
### 1.Description
给定一个长度为n的整数数组prices；prices[i]代表第i天股票的价格。
如果你最多能交易k次，即：买k次，卖k次。求出你最多能赚多少钱。
前提是：买入下一股的时候，必须将上一股卖掉。
### 2.Example
Input: [2,4,1], k = 2
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
### 3.Corner Case
n: 当n为0/1的时候，直接返回0；
k: 当k <= 0的时候，直接返回0；
   当k >= n的时候，将k赋值为n - 1；
赚不到钱则返回0；

## 二、题解
### Solution-1：Dynamic Programming
1.根据leetcode123，可以得到相同的状态转移方程：
dp[j][i] = max(dp[j][i - 1], dp[j - 1][m] + prices[i] - prices[m]);
其中：j:[1, k]  i:[0, n - 1]  m:[0, i]

2.申请(k + 1) * n的dp数组空间，答案就是dp[k][n - 1];
j从[1, k]遍历
i从[1, n - 1]遍历
m从[0, i]遍历
对于每次固定好的j和i，找出一个位置m，使得prices[m] - dp[j - 1][m]的值最小。

时间复杂度：O(k * n ^ 2)
空间复杂度：O(k * n)

3.Code(https://leetcode.com/submissions/detail/284743219/)

### Solution-2：Dynamic Programming + Optimization
1.从Solution-1的解法来看，对于当前(k, i)序列，假设通过O(n)的循环，找到了一个位置m1，使得表达式的值最小了，从而算出了当前(k, i)的答案。
接着迭代到当前(k, i + 1)序列，此时还需要O(n)的循环：找到当前序列一个位置m2，使得表达式的值最小。
由于我们在上一次循环中已经计算出了[0, i]范围内使得表达式最小的一个位置。当前(k, i + 1)序列，直接通过O(1)的时间就可以算出来，而不必继续遍历[0, i + 1]

2.所以，对于遍历当前i位置来说，可以直接访问上一次i - 1的计算结果。从而把整体的时间复杂度优化为O(k * n)

时间复杂度：O(k * n)
空间复杂度：O(k * n)

3.Code(https://leetcode.com/submissions/detail/284745998/)

### Solution-3：Dynamic Programming + Optimization
1.尝试调换两次循环的次序
2.进一步优化空间，将二维降成一维
3.当k >= n / 2的时候，也就意味着可以用leetcode-122的O(n)方法来解。即无限交易次数，orz。。。

时间复杂度：O(k * n)
空间复杂度：O(k)

3.Code(https://leetcode.com/submissions/detail/284749560/)
Code(https://leetcode.com/submissions/detail/284755984/)