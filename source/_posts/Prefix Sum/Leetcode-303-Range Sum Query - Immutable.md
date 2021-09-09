---
title: Leetcode-303.Range Sum Query - Immutable
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Prefix Sum
- Array
---

## 一、题意

### 0、题目链接
[303.Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；
给定两个索引i、j；
【输出】
求出子数组：nums[i, j]中所有元素的和；（包含nums[i]、nums[j]）
【要求】
无；

### 2、Example
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3

<!-- more -->

### 3、Corner Case
1. nums中的元素不会变化；
2. sumRange函数会被频繁调用；即：尽量降低sumRange的时间复杂度；
3. 0 <= n <= 10^4；
4. 当n == 0时，sumRange函数无意义，不应该被调用；
4. -10^5 <= nums[i] <= 10^5；
5. i、j均合法，且i <= j；

## 二、题解

### 0、思考
1. 考虑数据范围：
元素最多有10^4个，每个最大为10^5，最小为-10^5：任意sum一定在范围[-10^9, 10^9]内，不会爆signed int；

### 1、Solution-1：Brute Force
1. 初始化时，将nums数组保存起来。时间复杂度：O(n)；

2. 查询时，从i开始，遍历到j，计算sum。时间复杂度：O(n)；

3. Code(https://leetcode.com/submissions/detail/383614750/)
AC
时间复杂度：O(n)-初始化；O(n)-查询；
空间复杂度：O(n)-初始化；O(1)-查询；
```C++
class NumArray {
private:
    int len;
    vector<int> clone;
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        len = n;
        if (n == 0) {
            return;
        }
        
        clone = nums;
    }
    
    int sumRange(int i, int j) {
        if (len == 0) {
            return -1; // invalid
        }
        
        int sum = 0;
        for (int idx = i; idx <= j; idx++) {
            sum += clone[idx];
        }
        return sum;
    }
};
```

### 2、Solution-2：Prefix Sum
1. 使用前缀和的思想，来优化查询的时间复杂度为O(1)；

2. 思路：prefix[i]记录子数组nums[0, i - 1]的和；即：
prefix[0] = 0;
prefix[1] = nums[0] = prefix[0] + nums[0]
prefix[2] = nums[0] + nums[1] = prefix[1] + nums[1]
...
prefix[n] = nums[0] + nums[1] + ... + nums[n - 1] = prefix[n - 1] + nums[n - 1]
进一步，可以得出：
prefix[i] = nums[0] + nums[1] + ... + nums[i - 1] = prefix[i - 1] + nums[i - 1]

3. 初始化：为prefix数组赋值即可。时间复杂度：O(n)；

4. 查询：prefix[j + 1] - prefix[i]即可；时间复杂度：O(1)；

5. Code(https://leetcode.com/submissions/detail/383619415/)
AC
时间复杂度：O(n)-初始化；O(1)-查询；
空间复杂度：O(n)-初始化；O(1)-查询；
```C++
class NumArray {
private:
    int len;
    vector<int> prefix;
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        len = n;
        if (n == 0) {
            return;
        }
        
        prefix = vector<int>(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            prefix[i] = prefix[i - 1] + nums[i - 1];
        }
    }
    
    int sumRange(int i, int j) {
        if (len == 0) {
            return -1; // invalid
        }
        
        return prefix[j + 1] - prefix[i];
    }
};
```

