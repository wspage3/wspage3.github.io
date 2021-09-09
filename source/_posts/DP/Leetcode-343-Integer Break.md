---
title: Leetcode-343.Integer Break
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
- Greedy
---

## 一、题意

### 0、题目链接
[343.Integer Break](https://leetcode.com/problems/integer-break/)

### 1、Description
【输入】
给定一个正整数n；
【输出】
至少将n分成两个正整数，求其product，输出最大的product；
【要求】
无；

### 2、Example
Input: 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.

<!-- more -->

### 3、Corner Case
1. 2 <= n <= 58；
2. n == 2时，输出1；
3. n == 3时，输出2；
4. n == 4时，输出4；

## 二、题解

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：将i分成至少两个正整数，最大的product，

2. 答案：dp[n]；

3. 初始化
dp[0] = dp[1] = -1，不可能进行拆分；
dp[2] = 1；

4. 递推式：
dp[i] = max(dp[i], max(j, dp[j]) * max(i - j, dp[i - j]));
解释：将i分成j、i - j两部分；

5. Code(https://leetcode.com/submissions/detail/429889597/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1);
        dp[0] = -1;
        dp[1] = -1;
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            dp[i] = 0;
            // i = 3时，j: [1]
            // i = 4时，j: [1, 2]
            // i = 5时，j: [1, 2]
            // i = 6时，j: [1, 2, 3]
            for (int j = 1; j <= i / 2; j++) {
                dp[i] = max(dp[i], max(j, dp[j]) * max(i - j, dp[i - j]));
            }
        }
        return dp[n];
    }
};
```

### 2、Solution-2：Greedy
1. 当n <= 4的时候，直接输出答案；

2. 假设将n(n >= 5)分解成若干个数字，使得product最大：
若存在分解因子1：product不可能最大；
若存在分解因子5：product不可能最大：还不如将5继续分解成2 * 3；
若存在分解因子6：product不可能最大：还不如将6继续分解成3 * 3；
...

3. 结论
尽可能多得分解出3和2；
优先分解出3；

4. Code(https://leetcode.com/submissions/detail/429892421/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int integerBreak(int n) {
        if (n == 2) {
            return 1;
        } else if (n == 3) {
            return 2;
        } else if (n == 4) {
            return 4;
        }
        
        int ans = 1;
        while (n >= 5) {
            ans = ans * 3;
            n = n - 3;
        }
        // 走到此处，n一定为[2, 3, 4]中的其中一个。由于n已经至少被分割了一次，剩下的不用分割了，直接累乘就行。
        ans = ans * n;
        return ans;
    }
};
```