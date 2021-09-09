---
title: Leetcode-998.Maximum Binary Tree II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
- Array
- Monotonous Stack
---

## 一、题意

### 0、题目链接
[998.Maximum Binary Tree II](https://leetcode.com/problems/maximum-binary-tree-ii/)

### 1、Description
【输入】
给定一棵“最大二叉树”，根节点为root；
给定一个整数val；
【输出】
将val插入到给定的“最大二叉树”中，并保持“最大二叉树”的性质；
【要求】
无；

### 2、Example
Input: root = [4,1,3,null,null,2], val = 5
Output: [5,4,null,1,3,null,null,2]
Explanation: A = [1,4,2,3], B = [1,4,2,3,5]

<!-- more -->

### 3、Corner Case
1. 若root为空，返回以val构造的单结点；

## 二、题解

### 1、Solution-1：Recursive
1. 递归方式解决；

3. Code(https://leetcode.com/submissions/detail/419741826/)
AC
时间复杂度：O(n)； 
空间复杂度：O(n)； 
```C++
class Solution {
public:
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if (root == NULL) {
            return new TreeNode(val);
        }
        
        if (val > root->val) {
            TreeNode* ans = new TreeNode(val);
            ans->left = root;
            return ans;
        } else {
            root->right = insertIntoMaxTree(root->right, val);
            return root;
        }
    }
};
```

### 2、Solution-2：Iterative
1. 类似于【Leetcode-654.Maximum Binary Tree】中，逐一将元素插入“最大二叉树”；

2. Code(https://leetcode.com/submissions/detail/419736133/)
AC
时间复杂度：O(n)； 
空间复杂度：O(1)； 
```C++
class Solution {
public:
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if (root == NULL) {
            return new TreeNode(val);
        }
        
        if (val > root->val) {
            TreeNode* ans = new TreeNode(val);
            ans->left = root;
            return ans;
        } else {
            TreeNode* cur = root;
            TreeNode* pre = root;
            while (cur != NULL && cur->val > val) {
                pre = cur;
                cur = cur->right;
            }
            // 循环结束后，将ans插入到pre和cur中间即可；
            TreeNode* ans = new TreeNode(val);
            ans->left = cur;
            pre->right = ans;
            return root;
        }
    }
};
```