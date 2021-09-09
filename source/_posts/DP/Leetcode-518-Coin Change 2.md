---
title: Leetcode-518.Coin Change 2
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Combination
- DFS
- Backtracking
- Dynamic Programming
---

## 一、题意

### 黑板上排列组合，你舍得解开吗~~

### 0、题目链接
[518.Coin Change 2](https://leetcode.com/problems/coin-change-2/)

### 1、Description
【输入】
给定一个长度为n的整数数组coins；
coins性质：元素都是正数；无重复元素；
给定一个整数target；
target性质：非负数；
【输出】
从coins中取出一些元素，每个元素可以取无限次；
求：一共有多少种组合方式，使得取出的元素和为target；
组合：不关心元素间的顺序；
【要求】
无；

### 2、Example
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

<!-- more -->

### 3、Corner Case
1. target为非负数；
2. target == 0时：输出1；
3. 考虑target >= 1的情况；
4. coins[i]为正数；
5. coins中无重复数；
6. n == 0时：而target为正数，输出0；
7. 考虑n >= 1的情况；

## 二、题解

### 1、Solution-1：Backtracking
1. 对coins进行排序，为了在搜索过程中剪枝；

2. 当前路径合法：target为0了；

3. 当前路径非法：target为负数了；

4. 求组合Combination：
start从0开始；
为了去重，下一层“不走回头路”：
若允许每个元素取一次：下一层从start + 1开始；
若允许每个元素取无限次：下一层仍从start开始；

5. 求排列Permutation：
为了将顺序考虑进来，每一层均从nums[0]选择，到nums[n - 1]结束；
若允许每个元素取一次：使用visited数组标记路径上已经选择过的元素；
若允许每个元素取无限次：从0选择，到n - 1结束；

6. Code(https://leetcode.com/submissions/detail/429538723/)
TLE
// 每个结点的“度”为：n；
// 树高h为：(target / min(nums[i]))
时间复杂度：O(n ^ h)  
空间复杂度：O(h)
```C++
class Solution {
private:
    void backtracking(int start, int target, vector<int>& coins, int& ans) {
        if (target == 0) {
            ans++;
            return;
        }
        int n = coins.size();
        for (int i = start; i <= n - 1; i++) {
            if (coins[i] > target) {
                break;
            }
            backtracking(i, target - coins[i], coins, ans);
        }
    }
public:
    int change(int target, vector<int>& coins) {
        if (target == 0) {
            return 1;
        }
        int n = coins.size();
        if (n == 0) {
            return 0;
        }
        
        sort(coins.begin(), coins.end());
        int ans = 0;
        backtracking(0, target, coins, ans);
        return ans;
    }
};
```

### 2、Solution-2：Dynamic Programming
1. dp[i][j]表示：使用nums[0]到nums[i]，组成j，一共有多少种组合方式；

2. 答案：dp[n - 1][target]；

3. 初始化
第一列：j == 0的时候，dp[i][0] = 1;
第一行：若j % coins[0] == 0，则j能被组成，dp[0][j] = 1；否则dp[0][j] = 0；

4. 递推式
* 不用当前元素(coin)：dp[i - 1][j]；
* 用了当前元素(coin)：dp[i][j - coin];

5. Code(https://leetcode.com/submissions/detail/429572012/)
AC
时间复杂度：O(n * target)  
空间复杂度：O(n * target)
```C++
class Solution {
public:
    int change(int target, vector<int>& coins) {
        if (target == 0) {
            return 1;
        }
        int n = coins.size();
        if (n == 0) {
            return 0;
        }
        
        vector<vector<int>> dp(n, vector<int>(target + 1, 0));
        // 1. init
        dp[0][0] = 1;
        for (int j = 1; j <= target; j++) {
            dp[0][j] = j % coins[0] == 0 ? 1 : 0;
        }
        // 2. solve
        for (int i = 1; i <= n - 1; i++) {
            dp[i][0] = 1;
            int coin = coins[i];
            for (int j = 1; j <= target; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j < coin) {
                    continue;
                }
                dp[i][j] += dp[i][j - coin];
            }
        }
        // 3. return 
        return dp[n - 1][target];
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/429616738/)
AC
时间复杂度：O(n * target)  
空间复杂度：O(target)
```C++
class Solution {
public:
    int change(int target, vector<int>& coins) {
        if (target == 0) {
            return 1;
        }
        int n = coins.size();
        if (n == 0) {
            return 0;
        }
        
        vector<int> dp(target + 1);
        // 1. init
        dp[0] = 1;
        for (int j = 1; j <= target; j++) {
            dp[j] = j % coins[0] == 0 ? 1 : 0;
        }
        // 2. solve
        for (int i = 1; i <= n - 1; i++) {
            dp[0] = 1;
            int coin = coins[i];
            for (int j = 1; j <= target; j++) {
                if (j < coin) {
                    continue;
                }
                dp[j] += dp[j - coin];
            }
        }
        // 3. return
        return dp[target];
    }
};
```