---
title: Leetcode-226.Invert Binary Tree
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
[226.Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
将这棵二叉树翻转，返回翻转后的根结点；
翻转：对二叉树中的所有结点，将其左右儿子互换；
【要求】
递归、迭代；

### 2、Example
Input:  [1,2,3]
Output: [1,3,2]

<!-- more -->

### 3、Corner Case
1. 若root为NULL，无需翻转，直接返回NULL；

## 二、题解

### 1、Solution-1：DFS
1. 遍历顺序：以DFS的顺序（preorder）访问树中所有的结点；

2. 对于任一结点node来说：
将其左子树进行翻转，翻转后的根结点记为r；
将其右子树进行翻转，翻转后的根结点记为l；
修改node的左儿子为l，右儿子为r；

3. Code(https://leetcode.com/submissions/detail/415870398/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) {
            return NULL;
        }
        
        TreeNode* l = invertTree(root->right);
        TreeNode* r = invertTree(root->left);
        root->left = l;
        root->right = r;
        
        return root;
    }
};
```

### 2、Solution-2：BFS Using Queue
1. 遍历顺序：对二叉树进行层次遍历，自顶向下：
访问到某个结点node时：将其左右儿子互换即可；

2. Code(https://leetcode.com/submissions/detail/415872675/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) {
            return NULL;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode* cur = q.front();
            q.pop();
            TreeNode* temp = cur->left;
            cur->left = cur->right;
            cur->right = temp;
            if (cur->left != NULL) {
                q.push(cur->left);
            }
            if (cur->right != NULL) {
                q.push(cur->right);
            }
        }
        
        return root;
    }
};
```