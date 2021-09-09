---
title: Leetcode-108.Convert Sorted Array to Binary Search Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
- Array
---

## 一、题意

### 0、题目链接
[108.Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

### 1、Description
【输入】
给定一个严格升序数组nums；
【输出】
根据nums，构造一棵高度平衡的BST；
【要求】
无；

### 2、Example
Input:[-10,-3,0,5,9]
Output:[0,-3,9,-10,NULL,5,NULL]

<!-- more -->

### 3、Corner Case
1. 若nums为空，返回NULL；

## 二、题解

### 1、Solution-1：Recursive
1. 递归 + 二分构造；

2. Code(https://leetcode.com/submissions/detail/419137957/)
AC
时间复杂度：O(n)； // T(n) = 2 * T(n/2) = O(n)
空间复杂度：O(h)； // h = log n
```C++
class Solution {
private:
    TreeNode* helper(vector<int>& nums, int left, int right) {
        if (left > right) {
            return NULL;
        } 
        int mid = left + (right - left) / 2;
        TreeNode* node = new TreeNode(nums[mid], NULL, NULL);
        node->left = helper(nums, left, mid - 1);
        node->right = helper(nums, mid + 1, right);
        return node;
    }
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int n = nums.size();
        return helper(nums, 0, n - 1);
    }
};
```