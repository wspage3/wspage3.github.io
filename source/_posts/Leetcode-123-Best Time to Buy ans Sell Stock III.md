---
title: Leetcode123-Best Time to Buy and Sell Stock III
---

## 一、题意
题目链接: [Leetcode123-Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
### 1.Description
给定一个长度为n的整数数组prices；prices[i]代表第i天股票的价格。
如果你最多能交易2次，即：买2次，卖2次。求出你最多能赚多少钱。
前提是：买入下一股的时候，必须将上一股卖掉。
### 2.Example
Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
### 3.Corner Case
n：当n为0/1的时候，直接返回0；
赚不到钱则返回0；

## 二、题解
### Solution-1：峰谷法
1.将每天的价格画成折线图，折线图中上升的线段就是可以获得利润的时候。
由于我们最多可以交易两次，也是就说找到两条折线，其高度差最大。
所以说用变量maxProfit1和maxProfit2来维护这两个值，数组从左往右遍历一次，找出所有的valley和peak；然后依次更新两个最大值。
最后结果返回这两个值的和即可。

时间复杂度：O(n)
空间复杂度：O(1)

结果发现：pass了190/200的用例。
其中一个不过的用例是：[1,2,4,2,5,7,2,4,9,0]
按照我们上面的思路：找出所有的上升线段：1->2->4  2->5->7  2->4->9
挑两个最大的，计算出ans = 5 + 7 = 12
但实际上答案是13。分别是：1->7  2->9这两个区间。
通过观察，可以看出：1->2->4  2->5->7，这两个区间由于存在范围重叠，导致将其分开计算得到的两个最大值，比在一个计算得到的一个最大值要小。
这也是这种思路的问题所在。

3.Code(https://leetcode.com/submissions/detail/284240316/)

### Solution-2：Divide and Conquer
1.根据leetcode121的启示，我们可以找O(n)的时间内，找到一个区间[0, n - 1]交易一次所能获取的最大利润。
本题限定，我们最多能交易2次，所以对于位置i，我们可以先找到[0, i - 1]内交易一次所能获得的最大利润。以及[i, n - 1]内交易一次所能获得的最大利润。
然后把两部分加起来即可。在[0, n - 1]的范围内，找到一个i，使得其两部分的和最大。相当于进行两次leetcode121。

2.vector<int> l(n, 0);//l[i]代表：范围[0, i]内，交易一次，能获得的最大利润
  vector<int> r(n, 0);//r[i]代表：范围[i, n - 1]内，交易一次，能获得的最大利润
  则对于i在[0, n - 1]内，res[i] = l[i] + r[i]，找出最大的res[i]即可。

时间复杂度：O(n)
空间复杂度：O(n)

3.Code(https://leetcode.com/submissions/detail/284663137/)


### Solution-3：Dynamic Programming
1.状态转移：dp[k][i] = max(dp[k][i - 1], dp[k - 1][j] + prices[i] - prices[j])
其中dp[k][i]代表：最多交易k次，截止到第i天为止（包含第i天），所能获得的最多利润。则答案就是dp[k][n - 1]
分别遍历k和i即可。对于固定的k和i，找到一个j在[0, i]范围内，使得prices[j] - dp[k - 1][j]最小即可。
边界条件：
当k为0时，dp[k][i]都为0
当i为0时，dp[k][i]都为0

时间复杂度：O(k * n ^ 2)
空间复杂度：O(K * n)

结果TLE

3.Code(https://leetcode.com/submissions/detail/284681458/)

### Solution-4：Dynamic Programming + Optimization
1.Solution-3的思路是：我们对于一个给定的(k, i)，j在[0, i]中，寻找一个最小的j使得表达式（prices[j] - dp[k - 1][j]）的值最小。
找到之后，继续寻找(k, i + 1)..(k, i + 2)......的值，直到所有的k和i遍历完成。

2.通过观察，我们发现表达式（prices[j] - dp[k - 1][j]）的值只和位置j有关（在k固定的情况下）。
所以对于同一个k来说，当计算出了范围[0, i]内表达式的最小值后，计算[0, i + 1]只需要O(1)的时间。
也就是说，对于给定的(k, i)，我们保留(k, i - 1)的计算结果，通过递推来算出当前的结果。
而不是对于每一个(k, i)，我们通过遍历j来找出当下结果。
启示：当计算到某个状态时，而非仅仅考虑当前计算出来正确结果即可。尝试想一下当前结果对下一次的计算是否有用，有用的话将当前结果保留下来即可。

时间复杂度：O(k * n)
空间复杂度：O(K * n)

3.Code(https://leetcode.com/submissions/detail/284684804/)

### Solution-5：Dynamic Programming + Further Optimization
1.进一步，我们可以发现k是一个常数，所以尝试将两层循环倒换一下顺序，即：常数级别的k放在内层循环中。

时间复杂度：O(k * n)
空间复杂度：O(K * n)

Code(https://leetcode.com/submissions/detail/284698683/)

2.再进一步，我们可以发现dp[i][k]的值只和dp[i - 1][k]的值有关，而和i - 2, i - 3......无关。所以我们可以将第一维用一个变量表示即可，即将二维压缩为一维。空间复杂度变为：O(k)

时间复杂度：O(k * n)
空间复杂度：O(k)

Code(https://leetcode.com/submissions/detail/284701294/)





















