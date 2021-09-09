---
title: Leetcode-152.Maximum Product Subarray
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[152.Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
求出nums的所有非空子数组中，其product最大的那个，输出这个product；
【要求】
无；

### 2、Example
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

<!-- more -->

### 3、Corner Case
1. n == 0时，输出0；
2. n == 1时，输出nums[0]；

## 二、题解

### 0、思考
1. 对于一个长度为n的数组nums，其非空子数组有n * (n + 1) / 2个；

2. 暴力解法：O(n * n)；

### 1、Solution-1：Dynamic Programming
1. dpMax[i]表示：以nums[i]开头的所有子数组中，最大的product；
dpMin[i]表示：以nums[i]开头的所有子数组中，最小的product；

2. 答案
max(dpMax[0]-dpMax[n - 1])；

3. 初始化
dpMax[n - 1] = dpMin[n - 1] = nums[n - 1]；

4. 递推式
* nums[i] = 0：
dpMax[i] = 0;
dpMin[i] = 0;
* nums[i] > 0：
dpMax[i] = max(nums[i], nums[i] * dpMax[i + 1]);
dpMin[i] = min(nums[i], nums[i] * dpMin[i + 1]);
* nums[i] < 0：
dpMax[i] = max(nums[i], nums[i] * dpMin[i + 1]);
dpMin[i] = min(nums[i], nums[i] * dpMax[i + 1]);

5. Code(https://leetcode.com/submissions/detail/428795541/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        
        vector<int> dpMax(n);
        vector<int> dpMin(n);
        dpMax[n - 1] =dpMin[n - 1] = nums[n - 1];
        int ans = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] == 0) {
                dpMax[i] = dpMin[i] = 0;
            } else if (nums[i] > 0) {
                dpMax[i] = max(nums[i], nums[i] * dpMax[i + 1]);
                dpMin[i] = min(nums[i], nums[i] * dpMin[i + 1]);
            } else {
                dpMax[i] = max(nums[i], nums[i] * dpMin[i + 1]);
                dpMin[i] = min(nums[i], nums[i] * dpMax[i + 1]);
            }
            ans = max(ans, dpMax[i]);
        }
        return ans;
    }
};
```

6. 空间优化

7. Code(https://leetcode.com/submissions/detail/428807874/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        
        int nextMax = nums[n - 1];
        int nextMin = nums[n - 1];
        int ans = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            int curMax;
            int curMin;
            if (nums[i] == 0) {
                curMax = curMin = 0;
            } else if (nums[i] > 0) {
                curMax = max(nums[i], nums[i] * nextMax);
                curMin = min(nums[i], nums[i] * nextMin);
            } else {
                curMax = max(nums[i], nums[i] * nextMin);
                curMin = min(nums[i], nums[i] * nextMax);
            }
            ans = max(ans, curMax);
            nextMax = curMax;
            nextMin = curMin;
        }
        return ans;
    }
};
```