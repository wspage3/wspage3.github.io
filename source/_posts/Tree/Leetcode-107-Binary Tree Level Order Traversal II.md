---
title: Leetcode-107.Binary Tree Level Order Traversal II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
---

## 一、题意

### 0、题目链接
[107.Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
对二叉树进行层次遍历：输出每个结点的val；以二维数组的形式返回：每层对应一个一维数组；
先输出最后一层，最后输出第一层；
【要求】
无；

### 2、Example
Input: [3,9,20,null,null,15,7]
Output: 
[
  [15,7],
  [9,20],
  [3]
]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：BFS Using Queue
1. 执行正常的层次遍历；

2. 最后，将ans逆序即可；

3. Code(https://leetcode.com/submissions/detail/399465065/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> ans;
        if (root == NULL) {
            return ans;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            vector<int> level;
            int n = q.size();
            for (int i = 1; i <= n; i++) {
                TreeNode* node = q.front();
                q.pop();
                level.push_back(node->val);
                if (node->left != NULL) {
                    q.push(node->left);
                }
                if (node->right != NULL) {
                    q.push(node->right);
                }
            }
            ans.push_back(level);
        }
        
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```