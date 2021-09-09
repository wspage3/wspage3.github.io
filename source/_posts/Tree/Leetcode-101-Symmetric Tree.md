---
title: Leetcode-101.Symmetric Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
- DFS
- BFS
---

## 一、题意

### 0、题目链接
[101.Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
判断这棵二叉树是否是左右对称的；
【要求】
递归、迭代；

### 2、Example
Input:  [1,2,2,3,4,4,3]
Output: true

<!-- more -->

### 3、Corner Case
1. 若root为NULL，输出true；

## 二、题解

### 1、Solution-1：DFS
1. 以DFS的顺序（preorder）访问树中所有的结点；

2. 对于root来说，只要保证：
左子树与右子树为镜像关系；
则：以root为根的二叉树是左右对称的；

3. 辅助函数：isMirror(p, q)
判断p、q是否是镜像关系；

4. Code(https://leetcode.com/submissions/detail/414744989/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
private:
    bool isMirror(TreeNode* p, TreeNode* q) {
        if (p == NULL || q == NULL) {
            return p == NULL && q == NULL ? true : false;
        }
        if (p->val != q->val) {
            return false;
        }
        return isMirror(p->left, q->right) && isMirror(p->right, q->left);
    }
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) {
            return true;
        }
        return isMirror(root->left, root->right);
    }
};
```

### 2、Solution-2：BFS Using Queue
1. 对二叉树进行层次遍历：
每次比较对称位置上的两个结点；

2. Code(https://leetcode.com/submissions/detail/414771753/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) {
            return true;
        }
        
        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);
        while (!q.empty()) {
            TreeNode* node1 = q.front();
            q.pop();
            TreeNode* node2 = q.front();
            q.pop();
            if (node1 == NULL && node2 == NULL) {
                continue;
            } else if (node1 == NULL || node2 == NULL) {
                return false;
            } else {
                if (node1->val != node2->val) {
                    return false;
                }
                q.push(node1->left);
                q.push(node2->right);
                q.push(node1->right);
                q.push(node2->left);
            }
        }
        
        return true;
    }
};
```