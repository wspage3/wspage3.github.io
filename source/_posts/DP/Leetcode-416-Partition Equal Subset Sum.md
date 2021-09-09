---
title: Leetcode-416.Partition Equal Subset Sum
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[416.Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
判断是否可以将nums中元素分成两部分，两部分的和相等；
【要求】
无；

### 2、Example
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 200
2. n == 1时，输出false；
3. 若sum(nums)为奇数，输出false；
4. nums中元素均为正数；

## 二、题解

### 0、思考
1. 本题实质上是：找出nums的一个子序列，使得其和为sum / 2，且每个元素只能使用一次；

2. Leetcode-518-Coin Change 2
找出nums的一个子序列，使得其和为target，且每个元素能使用无限次；

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：能否从子串nums[0...i]中选择一个子序列，使得和为j；

2. 答案
target = sum / 2；
dp[0][target] || dp[1][target] || ... || dp[n - 1][target]

3. 初始化
第一列：j == 0的时候，dp[i][0] = 1;
第一行：若j == nums[0]，则j能被组成，dp[0][j] = 1；否则dp[0][j] = 0；

4. 递推式
* 不用当前元素(nums[i])：dp[i - 1][j]；
* 用了当前元素(nums[i])：dp[i - 1][j - nums[i]]；
本题：每个元素仅用一次，dp[i - 1][j - nums[i]]；
LC518：每个元素可用无限次，dp[i][j - nums[i]]；

5. Code(https://leetcode.com/submissions/detail/431660147/)
AC
时间复杂度：O(n * target)
空间复杂度：O(n * target)
```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return false;
        }
        
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (sum % 2 == 1) {
            return false;
        }
        
        int target = sum / 2;
        vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));
        // 1. init
        dp[0][0] = true;
        // 2. solve
        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            dp[i][0] = true;
            for (int j = 1; j <= target; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j - x >= 0) {
                    dp[i][j] = dp[i][j] || dp[i - 1][j - x];
                }
            }
            if (dp[i][target]) {
                return true;
            }
        }
        
        return false;
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/431661234/)
AC
时间复杂度：O(n * target)
空间复杂度：O(target)
```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return false;
        }
        
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (sum % 2 == 1) {
            return false;
        }
        
        int target = sum / 2;
        vector<bool> dp(target + 1, false);
        // 1. init
        dp[0] = true;
        // 2. solve
        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            dp[0] = true;
            for (int j = target; j >= 1; j--) {
                if (j - x >= 0) {
                    dp[j] = dp[j] || dp[j - x];
                }
            }
            if (dp[target]) {
                return true;
            }
        }
        
        return false;
    }
};
```