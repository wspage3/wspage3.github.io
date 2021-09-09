---
title: Leetcode-110.Balanced Binary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
---

## 一、题意

### 0、题目链接
[110.Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
判断二叉树是否是高度平衡的；
高度平衡：所有结点的左右子树的高度差不超过1；
【要求】
无；

### 2、Example
Input: root = [3,9,20,null,null,15,7]
Output: true

<!-- more -->

### 3、Corner Case
1. 若root == NULL，返回true；
2. 0 <= n <= 5000；

## 二、题解

### 1、Solution-1：DFS Using Recursion
1. Code(https://leetcode.com/submissions/detail/410934674/)
AC
时间复杂度：O(n)
空间复杂度：O(h) // h最坏情况下为n；
```C++
class Solution {
private:
    // 返回-1：以node为根结点的子树不平衡；
    // 返回非负数：以node为根结点的子树的高度（最大高度）；
    int helper(TreeNode* node) {
        if (node == NULL) {
            return 0;
        }
        
        int lDepth = helper(node->left);
        if (lDepth == -1) {
            return -1;
        }
        int rDepth = helper(node->right);
        if (rDepth == -1) {
            return -1;
        }
        
        if (abs(lDepth - rDepth) > 1) {
            return -1;
        } 
        
        return max(lDepth, rDepth) + 1;
    }
public:
    bool isBalanced(TreeNode* root) {
        return helper(root) >= 0;
    }
};
```