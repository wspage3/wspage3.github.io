---
title: Leetcode-307.Range Sum Query - Mutable
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Prefix Sum
- Array
- Binary Indexed Tree
---

## 一、题意

### 0、题目链接
[307.Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；
实现以下两种操作：
区间求和：sumRange(i, j)：给定两个索引i、j；
单点更新：update(i, val)：给定索引i、以及整数val；
【输出】
区间求和：sumRange(i, j)：求出子数组：nums[i, j]中所有元素的和；（包含nums[i]、nums[j]）
单点更新：update(i, val)：将nums[i]替换为val；
【要求】
无；

### 2、Example
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8

<!-- more -->

### 3、Corner Case
1. nums中的元素会变化，即通过update函数进行更新；
2. sumRange、update函数都会被频繁调用，且两者的调用次数均匀分布；
3. 当n == 0时，sumRange、update函数无意义，不应该被调用；
4. i、j均合法，且i <= j；

## 二、题解

### 0、思考
1. 假定数据范围同303题，不会爆signed int；

2. 直接暴力：
对于单点更新update，我们可以将nums保存下来，从而做到O(1)的时间复杂度；
但此时，sumRange的时间复杂度为O(n)；

3. 前缀和：
参考304题的思路，我们用prefix数组记录前缀和，从而可以使得sumRange的时间复杂度为O(1)；
但此时，update的时间复杂度为O(n)，因为每一个update都会修改若干个prefix；

4. 树状数组；

### 1、Solution-1：Brute Force
1. 228ms；

2. Code(https://leetcode.com/submissions/detail/383636234/)
AC
时间复杂度：O(n)-初始化；O(n)-区间求和；O(1)-单点更新；
空间复杂度：O(n)-初始化；O(1)-区间求和；O(1)-单点更新；
```C++
class NumArray {
private:
    int _n;
    vector<int> clone;
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        _n = n;
        if (n == 0) {
            return;
        }
        
        clone = nums;
    }
    
    void update(int i, int val) {
        if (_n == 0) {
            return; // invalid
        }
        
        clone[i] = val;
    }
    
    int sumRange(int i, int j) {
        if (_n == 0) {
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
1. 548ms；

2. Code(https://leetcode.com/submissions/detail/383641166/)
AC
时间复杂度：O(n)-初始化；O(1)-区间求和；O(n)-单点更新；
空间复杂度：O(n)-初始化；O(1)-区间求和；O(1)-单点更新；
```C++
class NumArray {
private:
    int _n;
    vector<int> clone;
    vector<int> prefix;
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        _n = n;
        if (n == 0) {
            return;
        }
        
        clone = nums;
        
        prefix = vector<int>(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            prefix[i] = prefix[i - 1] + nums[i - 1];
        }
    }
    
    void update(int i, int val) {
        if (_n == 0) {
            return; // invalid
        }
        
        clone[i] = val;
        for (int idx = i + 1; idx <= _n; idx++) {
            prefix[idx] = prefix[idx - 1] + clone[idx - 1];
        } 
    }
    
    int sumRange(int i, int j) {
        if (_n == 0) {
            return -1; // invalid
        }
        
        return prefix[j + 1] - prefix[i];
    }
};
```

### 3、Solution-3：Binary Indexed Tree
1. 52ms；

2. 树状数组的索引必须从1开始；

3. Code(https://leetcode.com/submissions/detail/383728680/)
AC
时间复杂度：O(n * log n)-初始化；O(log n)-区间求和；O(log n)-单点更新；
空间复杂度：O(n)-初始化；O(1)-区间求和；O(1)-单点更新；
```C++
class NumArray {
private:
    int _n;
    vector<int> tree;
    vector<int> clone;
    // 内部函数，x为内部tree的索引，即从1开始；
    // 求出nums[0, x - 1]的区间和；
    int query(int x) {
        int ans = 0;
        while (x) {
            ans += tree[x];
            x -= (x & -x);
        }
        return ans;
    }
    // 内部函数，i为内部tree的索引，即从1开始；
    // 将nums[i - 1]的值，加上val；
    void add(int i, int val) {       
        while (i <= _n) {
            tree[i] += val;
            i += (i & -i);
        }
    }
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        _n = n;
        if (n == 0) {
            return;
        }
        
        clone = nums;
        tree = vector<int>(n + 1, 0);
        
        for (int i = 0; i <= n - 1; i++) {
            add(i + 1, nums[i]);
        }
    }
    
    void update(int i, int val) {
        if (_n == 0) {
            return; // invalid
        }
        
        int gap = val - clone[i];
        clone[i] = val;
        
        add(i + 1, gap);
    }
    
    int sumRange(int i, int j) {
        if (_n == 0) {
            return -1; // invalid
        }
        
        return query(j + 1) - query(i);
    }
};
```