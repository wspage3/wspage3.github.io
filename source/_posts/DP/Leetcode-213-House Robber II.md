---
title: Leetcode-213.House Robber II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[213.House Robber II](https://leetcode.com/problems/house-robber-ii/)

### 1、Description
【输入】
给定一个长度为n的数组nums，代表着n户人家的财产；
【输出】
小王今晚打算去打劫这n户人家；
已知：若相邻的两户人家同时被打劫，则会自动触发警报；
同时：这n户人家围成一个圈，也就是说，第0户人家和第n-1户人家是邻居；
求小王今晚最多能打劫多少财产，输出这个值；
【要求】
无；

### 2、Example
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 100
2. n == 0时，输出0；
3. n == 1时，输出nums[0]；
4. 0 <= nums[i] <= 1000

## 二、题解

### 0、思考
1. 相较于Leetcode-198-House Robber，本题多了一个条件：第0户人家和第n-1户人家是邻居；

2. 根据这一条件，将所有打劫的方式分为三类：
* 抢nums[0]，不抢nums[n - 1]：rob(nums, 0, n - 2);
* 抢nums[n - 1]，不抢nums[0]：rob(nums, 1, n - 1);
* 不抢nums[0]，不抢nums[n - 1]：rob(nums, 1, n - 2);
由于nums中元素均为非负数，所以:
rob(nums, 1, n - 2) <= rob(nums, 1, n - 1);
rob(nums, 1, n - 2) <= rob(nums, 0, n - 2);

3. 综上所述，只要分别求出上面两种情况，然后取较大值，即要求的答案；

### 1、Solution-1：Dynamic Programming
1. Code(https://leetcode.com/submissions/detail/428217809/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
private:
    int helper(vector<int>& nums, int l, int r) {
        // 调用者保证：r >= l
        if (l == r) {
            return nums[l];
        }
        
        int prepre = 0;
        int pre = 0;
        for (int i = l; i <= r; i++) {
            int cur = max(pre, nums[i] + prepre);
            prepre = pre;
            pre = cur;
        }
        return pre;
    }
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        if (n == 1) {
            return nums[0];
        }
        
        return max(helper(nums, 0, n - 2), helper(nums, 1, n - 1));
    }
};
```