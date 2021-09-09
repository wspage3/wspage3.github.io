---
title: Leetcode-098.Validate Binary Search Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Stack
- DFS
---

## 一、题意

### 0、题目链接
[098.Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
判断二叉树是否为二叉搜索树；
【要求】
递归、迭代；

### 2、Example
Input: [2,1,3]
Output: true

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出true；

## 二、题解

### 1、Solution-1：Recursive(Preorder)
1. 递归形式的前序遍历；

2. 辅助函数：help(node, left, right)
当访问到node时，(left, right)代表node的父结点期望node的取值范围；

3. Code(https://leetcode.com/submissions/detail/416650197/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
private:
    bool helper(TreeNode* node, long long left, long long right) {
        if (node == NULL) {
            return true;
        }
        if (node->val <= left || node->val >= right) {
            return false;
        }
        return helper(node->left, left, node->val)
            && helper(node->right, node->val, right);
    }
public:
    bool isValidBST(TreeNode* root) {
        return helper(root, LLONG_MIN, LLONG_MAX);
    }
};
```

### 2、Solution-2：Recursive(Inorder)
1. 递归形式的中序遍历；

2. pre：记录上一个结点的val值；
若本结点的val值小于等于pre：则不满足BST的性质；

3. Code(https://leetcode.com/submissions/detail/416663430/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
private:
    long long pre = LLONG_MIN;
public:
    bool isValidBST(TreeNode* root) {
        if (root == NULL) {
            return true;
        }
        if (isValidBST(root->left) == false) {
            return false;
        }
        if (root->val <= pre) {
            return false;
        }
        pre = root->val;
        return isValidBST(root->right);
    }
};
```

### 3、Solution-3：Iterative(Inorder)
1. 使用栈来模拟中序遍历；

2. Code(https://leetcode.com/submissions/detail/416914202/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (root == NULL) {
            return true;
        }
        
        TreeNode* cur = root;
        stack<TreeNode*> s;
        
        long long pre = LLONG_MIN;
        
        while (cur != NULL || !s.empty()) {
            while (cur != NULL) {
                s.push(cur);
                cur = cur->left;
            }
            TreeNode* node = s.top();
            if (node->val <= pre) {
                return false;
            }
            pre = node->val;
            s.pop();
            cur = node->right;
        }
        
        return true;
    }
};
```