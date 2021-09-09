---
title: Leetcode-437.Path Sum III
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
---

## 一、题意

### 0、题目链接
[437.Path Sum III](https://leetcode.com/problems/path-sum-iii/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
给定一个整数sum；
【输出】
求出满足条件的路径个数，路径上的所有结点的和恰好为给定的sum；
路径不要求从根结点出发，到叶结点结束，只要是竖直方向的即可；
【要求】
无；

### 2、Example
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
Return 3. The paths that sum to 8 are:
1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11

<!-- more -->

### 3、Corner Case
1. n <= 1000
2. -1000000 <= val <= 1000000
3. 若root == NULL，答案为0；

## 二、题解

### 1、Solution-1：Dual DFS
1. 当我们寻找以root为根结点的答案时，有两种情况：
路径不包含root本身；
路径包含root本身；

2. Code(https://leetcode.com/submissions/detail/421904610/)
AC
时间复杂度：O(n * n) 
空间复杂度：O(n) 
```C++
class Solution {
private:
    int pathSumFrom(TreeNode* node, int sum) {
        if (node == NULL) {
            return 0;
        }
        return (node->val == sum ? 1 : 0) 
             + pathSumFrom(node->left, sum - node->val) 
             + pathSumFrom(node->right, sum - node->val);
    }
public:
    int pathSum(TreeNode* root, int sum) {
        if (root == NULL) {
            return 0;
        }
        return pathSumFrom(root, sum)
             + pathSum(root->left, sum) 
             + pathSum(root->right, sum);
    }
};
```

### 2、Solution-2：Prefix Sum + Hash Table
to do