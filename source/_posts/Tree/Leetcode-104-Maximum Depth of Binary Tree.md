---
title: Leetcode-104.Maximum Depth of Binary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
- DFS
---

## 一、题意

### 0、题目链接
[104.Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
求二叉树的最大深度；
本题中，root的深度为1；
【要求】
无；

### 2、Example
Input: [3,9,20,null,null,15,7]
Output: 3

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出0；

## 二、题解

### 1、Solution-1：DFS Using Recursion
1. 递归结束：若当前子树为NULL，深度为0；

2. 对于任一结点来说：
递归求其左、右子树的深度，取(较大值 + 1)作为本子树的深度返回即可；

3. Code(https://leetcode.com/submissions/detail/410620594/)
AC
时间复杂度：O(n)
空间复杂度：O(h) // h最坏情况下为n；
```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

### 2、Solution-2：BFS Using Queue
1. Code(https://leetcode.com/submissions/detail/410621695/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        
        int ans = 0;
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            int size = q.size();
            for (int num = 1; num <= size; num++) {
                TreeNode* cur = q.front();
                q.pop();
                if (cur->left != NULL) {
                    q.push(cur->left);
                }
                if (cur->right != NULL) {
                    q.push(cur->right);
                }
            }
            ans++;
        }
        
        return ans;
    }
};
```