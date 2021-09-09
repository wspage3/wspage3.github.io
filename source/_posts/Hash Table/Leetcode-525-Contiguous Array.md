---
title: Leetcode-525.Contiguous Array
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Hash Table
- Prefix Sum
---

## 一、题意

### 0、题目链接
[525.Contiguous Array](https://leetcode.com/problems/contiguous-array/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；每个数字要么是0，要么是1；
【输出】
求出一个最长的子数组：有着相同数量的0和1。返回这个长度；
【要求】
无；

### 2、Example
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.

<!-- more -->

### 3、Corner Case
1. n == 0时，返回0即可；
2. n == 1时，返回0即可；
3. 考虑n >= 2的情况；
4. 题目保证n <= 50000；

## 二、题解

### 0、思考
1. 对于一个长度为n的数组来说，非空子数组的个数为：n * (n + 1) / 2；复杂度：O(n * n)

### 1、Solution-1：Brute Force
1. 遍历nums的所有非空子数组，找出其中满足条件的，求出其长度。

2. i从0开始，到n - 1结束；j从i开始，到n - 1结束；分别考虑子数组[i, j]是否满足条件。

3. Code(https://leetcode.com/submissions/detail/335177288/)
TLE
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return 0;
        }
        
        int maxLen = 0;
        for (int i = 0; i <= n - 1; i++) {
            int cnt0 = 0;
            for (int j = i; j <= n - 1; j++) {
                if (nums[j] == 0) {
                    cnt0++;
                }
                int len = j - i + 1;
                if (cnt0 * 2 == len) {
                    maxLen = max(maxLen, len);
                }
            }
        }
        return maxLen;
    }
};
```

### 2、Solution-2：Prefix Sum + Hash Table
1. 首先，把nums中的0替换为-1；

2. 这样，问题转化为：求nums的最大子数组的长度，其sum == 0；

3. 观察：
假设遍历到数组第i个元素，此时的sum为si，则si == a0 + a1 + a2 + ... + ai
假设遍历到数组第j个元素，此时的sum为sj，则sj == a0 + a1 + a2 + ... + ai + ... + aj 
若si == sj，则代表a(i + 1) + ... + aj这个子数组的和为0，也就是说这个子数组中-1和1的个数相同；

4. i从0开始遍历，到n - 1结束：
每次先求出[0, i]的前缀和sumI；
若sumI不在哈希表m中，则将{sumI, i}记录在哈希表m中；
若sumI已在哈希表m中，则更新maxLen = max(maxLen, i - m[sumI])即可；不需要更新m[sumI]的值为i，因为此时m[sumI]一定小于i；

5. 初始化：m[0] = -1。
当在位置i的时候，发现sumI为0，则代表[0, i]是一个满足条件的子数组，其长度：i - (-1) = i + 1；

6. Code(https://leetcode.com/submissions/detail/335185465/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return 0;
        }
        
        for (int i = 0; i <= n - 1; i++) {
            if (nums[i] == 0) {
                nums[i]--;
            }
        }
        
        int maxLen = 0;
        int sum = 0;
        unordered_map<int, int> m;
        m[0] = -1;
        
        for (int i = 0; i <= n - 1; i++) {
            sum += nums[i];
            if (m.find(sum) != m.end()) {
                maxLen = max(maxLen, i - m[sum]);
            } else {
                m[sum] = i;
            }
        }
        return maxLen;
    }
};
```