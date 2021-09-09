---
title: Leetcode-111.Minimum Depth of Binary Tree
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
[111.Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
求二叉树的最小深度；
本题中，root的深度为1；
【要求】
无；

### 2、Example
Input: [3,9,20,null,null,15,7]
Output: 2

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出0；

## 二、题解

### 1、Solution-1：DFS Using Recursion
1. 递归结束：若当前子树为NULL，深度为0；

2. 对于任一结点来说：
递归求其左、右子树的深度lAns、rAns；

3. Code(https://leetcode.com/submissions/detail/410664552/)
AC
时间复杂度：O(n)
空间复杂度：O(h) // h最坏情况下为n；
```C++
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        
        int lAns = minDepth(root->left);
        int rAns = minDepth(root->right);
        
        int ans;
        if (lAns && rAns) {
            ans = min(lAns, rAns) + 1;
        } else {
            ans = (lAns == 0 ? rAns + 1 : lAns + 1);
        }
        
        return ans;
    }
};
```

### 2、Solution-2：BFS Using Queue
1. Code(https://leetcode.com/submissions/detail/410667832/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        
        int ans = 0;
        while (!q.empty()) {
            ans++;
            int size = q.size();
            for (int num = 1; num <= size; num++) {
                TreeNode* cur = q.front();
                q.pop();
                if (cur->left == NULL && cur->right == NULL) {
                    return ans;
                }
                if (cur->left != NULL) {
                    q.push(cur->left);
                }
                if (cur->right != NULL) {
                    q.push(cur->right);
                }
            }
        }
        
        return ans;
    }
};
```