---
title: Leetcode-230.Kth Smallest Element in a BST
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
[230.Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

### 1、Description
【输入】
给定一棵二叉搜索树，根结点为root；
给定一个整数k；
【输出】
求出该二叉搜索树中，第k大的数字，并返回；
【要求】
无；

### 2、Example
Input: root = [3,1,4,null,2], k = 1
Output: 1

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 10000
2. k保证合法：1 <= k <= n

## 二、题解

### 1、Solution-1：Recursive(Inorder)
1. 递归形式的中序遍历；

2. Code(https://leetcode.com/submissions/detail/416956627/)
AC
时间复杂度：O(n) // 找到答案后，继续搜索，直到遍历完所有结点
空间复杂度：O(h)
```C++
class Solution {
private:
    int ans = 0;
    int cnt;
    void helper(TreeNode* node) {
        if (node->left != NULL) {
            helper(node->left);
        }
        cnt--;
        if (cnt == 0) {
            ans = node->val;
        }
        if (node->right != NULL) {
            helper(node->right);
        }
    }
public:
    int kthSmallest(TreeNode* root, int k) {
        cnt = k;
        helper(root);
        return ans;
    }
};
```

### 2、Solution-2：Iterative(Inorder)
1. 迭代形式的中序遍历；

2. Code(https://leetcode.com/submissions/detail/416951535/)
AC
时间复杂度：O(n) // 只要找到答案，就停止搜索
空间复杂度：O(h)
```C++
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        int cnt = 0;
        stack<TreeNode*> s;
        TreeNode* cur = root;
        while (cur != NULL || !s.empty()) {
            while (cur != NULL) {
                s.push(cur);
                cur = cur->left;
            }
            TreeNode* node = s.top();
            s.pop();
            cnt++;
            if (cnt == k) {
                return node->val;
            }
            cur = node->right;
        }
        return -1; // cannot execute
    }
};
```