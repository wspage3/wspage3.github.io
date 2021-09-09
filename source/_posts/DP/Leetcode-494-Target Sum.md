---
title: Leetcode-494.Target Sum
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
- Backtracking
---

## 一、题意

### 0、题目链接
[494.Target Sum](https://leetcode.com/problems/target-sum/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
给定一个整数target；
【输出】
对于nums的每个元素，可以修改为-nums[i]，也可以保持nums[i]；
求有多少种不同的方案，使得sum(nums)等于target；
【要求】
无；

### 2、Example
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
There are 5 ways to assign symbols to make the sum of nums be target 3.

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 20
2. nums中元素均为非负数；
3. n == 0时：若target为0，答案为1；否则，答案为0；
4. 若target > sum(nums) || target < -sum(nums)，则输出0； 

## 二、题解

### 1、Solution-1：Backtracking
1. 每个元素有两种选择：正、负；

2. 当所有元素都考虑过后，可以进行判断：
* target == 0，说明当前路径合法，计数，结束dfs；
* target != 0，说明当前路径非法，结束dfs；

3. Code(https://leetcode.com/submissions/detail/431628751/)
AC
时间复杂度：O(2 ^ n)
空间复杂度：O(n)
```C++
class Solution {
private:
    void backtracking(int pos, int target, vector<int>& nums, int& ans) {
        int n = nums.size();
        if (pos == n) {
            // n == 0时，直接进行判断target；
            if (target == 0) {
                ans++;
            }
            return;
        }
        
        // choose (+)nums[pos]
        backtracking(pos + 1, target - nums[pos], nums, ans);
        // choose (-)nums[pos]
        backtracking(pos + 1, target + nums[pos], nums, ans);
    }
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (target > sum || target < -sum) {
            return 0;
        }
        
        int ans = 0;
        backtracking(0, target, nums, ans);
        return ans;
    }
};
```

### 2、Solution-2：Dynamic Programming
1. Leetcode-416-Partition Equal Subset Sum
找出nums的一个子序列，使得其和为sum / 2，且每个元素只能使用一次；
本质上是一个经典的0/1背包问题；
且物品值、背包值均为正数；

2. 分析本题
pos为进行选择后，nums中非负元素的集合；
neg为进行选择后，nums中负元素的集合；
根据题意，得出以下等式：
s(pos) - s(neg) = target
s(pos) - s(neg) + s(pos) + s(neg) = target + s(nums)
2 * s(pos) = target + s(nums)
问题转化为：从nums中选择一些元素，每个元素只能选择一次，使得其和 = (target + s(nums)) / 2；
0/1背包问题：物品值、背包值均是非负数；

3. Code(https://leetcode.com/submissions/detail/431646875/)
AC
target = [target + s(nums)] / 2
时间复杂度：O(n * target)
空间复杂度：O(n * target)
```C++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (target > sum || target < -sum) {
            return 0;
        }
        
        //  分析：
        //  s(pos) - s(neg) = target
        //  s(pos) - s(neg) + s(pos) + s(neg) = target + s(nums)
        //  2 * s(pos) = target + s(nums)
        //  问题转化为：从nums中挑出一些元素，使得其sum为[target + s(nums)] / 2；
        //  经典的0/1背包问题，同Leetcode-416-Partition Equal Subset Sum
 
        // 若奇数，则不可能组成
        if ((target + sum) % 2 == 1) {
            return 0;
        }
 
        target = (target + sum) / 2;
        int n = nums.size();
        vector<vector<int>> dp(n + 1, vector<int>(target + 1, 0));
        // 1. init
        dp[0][0] = 1;
        // 2. calculate 1 ~ n - 1 row
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= target; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j >= nums[i - 1]) {
                    dp[i][j] += dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        
        return dp[n][target];
    }
};
```

4. 空间优化

5. Code(https://leetcode.com/submissions/detail/431656446/)
AC
target = [target + s(nums)] / 2
时间复杂度：O(n * target)
空间复杂度：O(target)
```C++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (target > sum || target < -sum) {
            return 0;
        }
        
        //  分析：
        //  s(pos) - s(neg) = target
        //  s(pos) - s(neg) + s(pos) + s(neg) = target + s(nums)
        //  2 * s(pos) = target + s(nums)
        //  问题转化为：从nums中挑出一些元素，使得其sum为[target + s(nums)] / 2；
        //  经典的0/1背包问题，同Leetcode-416-Partition Equal Subset Sum
 
        // 若奇数，则不可能组成
        if ((target + sum) % 2 == 1) {
            return 0;
        }
         
        target = (target + sum) / 2;
        int n = nums.size();
        vector<int> dp(target + 1, 0);
        // 1. init
        dp[0] = 1;
        // 2. calculate 1 ~ n - 1 row
        for (int i = 1; i <= n; i++) {
            for (int j = target; j >= 0; j--) {
                if (j >= nums[i - 1]) {
                    dp[j] += dp[j - nums[i - 1]];
                }
            }
        }
        
        return dp[target];
    }
};
```