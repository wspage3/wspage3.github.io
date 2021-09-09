---
title: Leetcode-377.Combination Sum IV
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Permutation
- DFS
- Backtracking
- Dynamic Programming
---

## 一、题意

### 黑板上排列组合，你舍得解开吗~~

### 0、题目链接
[377.Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；
nums性质：元素都是正数；无重复元素；
给定一个整数target；
target性质：正数；
【输出】
从nums中取出一些元素，每个元素可以取无限次；
求：一共有多少种排列方式，使得取出的元素和为target；
排列：关心元素间的顺序；
【要求】
无；

### 2、Example
nums = [1, 2, 3]
target = 4
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Output = 7

<!-- more -->

### 3、Corner Case
1. target为正数；
2. nums[i]为正数；
3. nums中无重复数；
4. n == 0时：而target为正数，输出0；
5. 考虑n >= 1的情况；

## 二、题解

### 1、Solution-1：Backtracking
1. 对nums进行排序，为了在搜索过程中剪枝；

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

6. Code(https://leetcode.com/submissions/detail/429538271/)
TLE
// 每个结点的“度”为：n；
// 树高h为：(target / min(nums[i]))
时间复杂度：O(n ^ h)  
空间复杂度：O(h)
```C++
class Solution {
private:
    void backtracking(int target, vector<int>& nums, int& ans) {
        if (target == 0) {
            ans++;
            return;
        }
        
        int n = nums.size();
        for (int i = 0; i <= n - 1; i++) {
            if (nums[i] > target) {
                break;
            }
            backtracking(target - nums[i], nums, ans);
        }
    }
public:
    int combinationSum4(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        
        int ans = 0;
        sort(nums.begin(), nums.end());
        backtracking(target, nums, ans);
        return ans;
    }
};
```

### 2、Solution-2：Dynamic Programming
1. dp[i]表示：使用nums中的全部元素，组成i，一共有多少种排列方式；

2. 答案：dp[target]；

3. 初始化
dp[0] = 1；
当i在nums出现的时候，此时：dp[i]累加dp[i - num]，即dp[i] += 1；

4. 递推式
dp[i] += dp[i - num]；

5. 注意
当target为1000，而nums = [3, 33, 333]这种情况时：
显然答案为0，但是当我们计算dp[999]的时候，加法运算会溢出int32，或long long，导致程序报错；
解决：将dp修改为unsigned int即可，加法溢出不会报错；

6. Code(https://leetcode.com/submissions/detail/429538435/)
AC
时间复杂度：O(target * n)  
空间复杂度：O(target)
```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {       
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        
        sort(nums.begin(), nums.end());
        
        vector<unsigned int> dp(target + 1, 0);
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int num : nums) {
                if (num > i) {
                    break;
                }
                dp[i] += dp[i - num];
            }
        }
        return dp[target];
    }
};
```