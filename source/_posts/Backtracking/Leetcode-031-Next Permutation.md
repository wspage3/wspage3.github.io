---
title: Leetcode-031.Next Permutation
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Permutation
- Array
---

## 一、题意

### 黑板上排列组合，你舍得解开吗~~

### 0、题目链接
[031.Next Permutation](https://leetcode.com/problems/next-permutation/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；代表一个【排列】；
nums性质：元素可能重复；
【输出】
求出下一个【排列】；
【要求】
就地操作，即：O(1)的空间复杂度；答案存储在nums中；

### 2、Example
[1,2,3] → [1,3,2]
[3,2,1] → [1,2,3]
[1,1,5] → [1,5,1]
[1,3,1,2] → [1,3,2,1]

<!-- more -->

### 3、Corner Case
1. n == 0 || n == 1时，直接return；
2. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. 047.Permutations II中：从nums中拿出全部数字；求所有【排列】；nums无重复元素；
例如：nums = [3,1,2,1]
ans = [
    [1,1,2,3],
    [1,1,3,2],
    [1,2,1,3],
    [1,2,3,1],
    [1,3,1,2],
    [1,3,2,1],
    [2,1,1,3],
    [2,1,3,1],
    [2,3,1,1],
    [3,1,1,2],
    [3,1,2,1],
    [3,2,1,1]
]

2. 本题中，即给定上述ans中的任一【排列】，求它的【下一个排列】。

3. 观察：ans中的元素顺序是字典序从小到大排列的。
即，【下一个排列】的定义是：给定数字序列的字典序中下一个更大的排列。
如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即非降序排列）。

### 1、Solution-1
1. 算法过程：
首先：从右往左，找第一个pos，使得：nums[pos] < nums[pos + 1]。
若pos == -1，说明nums此时已经是有序的（非升序排列）；对其进行reverse即可；
其次：从右往左，找第一个i，使得i >= pos + 1 && nums[i] > nums[pos]。
最后：交换nums[pos]、nums[i]；对区间[pos + 1, n - 1]进行reverse；

2. 举例：
nums = [2,3,1,1]
首先：pos = 0
其次：i = 1
最后：[3,2,1,1] → [3,1,1,2]

3. Code(https://leetcode.com/submissions/detail/302023443/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return;
        }
 
        int pos;
        for (pos = n - 2; pos >= 0; pos--) {
            if (nums[pos] < nums[pos + 1]) {
                break;
            }
        }
 
        if (pos == -1) {
            reverse(nums.begin(), nums.end());
        } else {
            int i;
            for (i = n - 1; i >= pos + 1; i--) {
                if (nums[i] > nums[pos]) {
                    break;
                }
            }
            swap(nums[pos], nums[i]);
            reverse(nums.begin() + pos + 1, nums.end());
        }
    }
};
```

