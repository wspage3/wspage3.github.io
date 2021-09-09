---
title: Leetcode-112.Path Sum
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
- Stack
- DFS
- BFS
---

## 一、题意

### 0、题目链接
[112.Path Sum](https://leetcode.com/problems/path-sum/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
给定一个整数sum；
【输出】
判断本二叉树中，是否存在一条从根结点到叶结点的路径，路径上的所有结点的和恰好为给定的sum；
【要求】
无；

### 2、Example
Input:  [1,2,3], sum = 3
Output: true

<!-- more -->

### 3、Corner Case
1. 若root为NULL，无论sum是多少，都不存在，返回false；

## 二、题解

### 1、Solution-1：DFS(Recursive)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. Code(https://leetcode.com/submissions/detail/415878805/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL) {
            return false;
        }
        
        if (root->left == NULL && root->right == NULL && root->val == sum) {
            return true;
        }
        
        return hasPathSum(root->left, sum - root->val) 
            || hasPathSum(root->right, sum - root->val);
    }
};
```

### 2、Solution-2：DFS(Iterative, using stack)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. 用stack模拟perorder的遍历过程；
遍历到某个结点node时，vs辅助栈记录从root到node路径上的和；

3. Code(https://leetcode.com/submissions/detail/415889785/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL) {
            return false;
        }
        
        stack<TreeNode*> s;
        s.push(root);
        stack<int> vs;
        vs.push(root->val);
        
        while (!s.empty()) {
            TreeNode* cur = s.top();
            s.pop();
            int curVal = vs.top();
            vs.pop();
            if (cur->left == NULL && cur->right == NULL && curVal == sum) {
                return true;
            }
            if (cur->right != NULL) {
                s.push(cur->right);
                vs.push(curVal + cur->right->val);
            }
            if (cur->left != NULL) {
                s.push(cur->left);
                vs.push(curVal + cur->left->val);
            }
        }
        
        return false;
    }
};
```

### 3、Solution-3：BFS(Iterative, using queue)
1. 遍历顺序：BFS的顺序（level-order）访问二叉树中的所有结点；

2. 用queue模拟level-order的遍历过程；
遍历到某个结点node时，vq辅助队列记录从root到node路径上的和；

3. Code(https://leetcode.com/submissions/detail/415883877/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL) {
            return false;
        }
        
        queue<TreeNode*> q;
        queue<int> vq;
        q.push(root);
        vq.push(root->val);
        
        while (!q.empty()) {
            TreeNode* cur = q.front();
            q.pop();
            int curVal = vq.front();
            vq.pop();
            if (cur->left == NULL && cur->right == NULL && curVal == sum) {
                return true;
            }
            if (cur->left != NULL) {
                q.push(cur->left);
                vq.push(curVal + cur->left->val);
            }
            if (cur->right != NULL) {
                q.push(cur->right);
                vq.push(curVal + cur->right->val);
            }
        }
        
        return false;
    }
};
```