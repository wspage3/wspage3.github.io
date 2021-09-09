---
title: Leetcode-235.Lowest Common Ancestor of a Binary Search Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
---

## 一、题意

### 0、题目链接
[235.Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

### 1、Description
【输入】
给定一棵BST，根结点为root；
给定上述BST中的两个结点p、q；
【输出】
求出p、q的LCA；
【要求】
无；

### 2、Example
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2

<!-- more -->

### 3、Corner Case
1. 2 <= n <= 100000
2. BST中没有val值相同的结点；
3. p != q
4. p、q一定在上述BST中；

## 二、题解

### 0、思考
1. 遍历方向：自顶向下，从root开始；

2. root遍历到某个结点时，通过比较p、q、root三者的大小：
若p、q均比root小，root转移到其左儿子；
若p、q均比root大，root转移到其右儿子；
若p、q一个大于root，一个小于root、或者二者有和root相等的，root则为答案，停止遍历；

### 1、Solution-1：DFS Using Recursion
1. Code(https://leetcode.com/submissions/detail/421286318/)
AC
时间复杂度：O(n) // 最坏情况下，遍历所有结点 
空间复杂度：O(h) // 最坏情况下，h = n
```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (p->val < root->val && q->val < root->val) {
            return lowestCommonAncestor(root->left, p, q);
        }
        if (p->val > root->val && q->val > root->val) {
            return lowestCommonAncestor(root->right, p, q);
        }
        return root;
    }
};
```

### 2、Solution-2：DFS Using Iterative
1. Code(https://leetcode.com/submissions/detail/421287020/)
AC
时间复杂度：O(n) // 最坏情况下，遍历所有结点 
空间复杂度：O(h) // 最坏情况下，h = n
```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while ((p->val - root->val) * (q->val - root->val) > 0) {
            root = (p->val < root->val) ? root->left : root->right;
        }
        return root;
    }
};
```