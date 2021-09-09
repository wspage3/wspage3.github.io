---
title: Leetcode-105.Construct Binary Tree from Preorder and Inorder Traversal
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
- Array
---

## 一、题意

### 0、题目链接
[105.Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### 1、Description
【输入】
给定两个整数数组preorder、inorder，长度均为n；
分别代表二叉树的前序、中序遍历序列；
每个数组中均没有重复元素，即二叉树中没有相同val的结点；
【输出】
构建上述二叉树，返回根结点；
【要求】
无；

### 2、Example
Input: 
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]

<!-- more -->

### 3、Corner Case
1. 特判二叉树为NULL的情况；

## 二、题解

### 1、Solution-1：DFS Using Recursion
1. Code(https://leetcode.com/submissions/detail/420816727/)
AC
时间复杂度：O(n * n) // 最坏情况下：二叉树没有右儿子，每次查找x的索引均为O(n)；
空间复杂度：O(h) // 最坏情况下：h = n
```C++
class Solution {
private:
    TreeNode* helper(vector<int>& preorder, int pl, int pr,
                     vector<int>& inorder, int il, int ir) {
        if (pl > pr) {
            return NULL;
        } else if (pl == pr) {
            return new TreeNode(preorder[pl]);
        }
        
        int x = preorder[pl];
        int i;
        for (i = il; i <= ir; i++) {
            if (inorder[i] == x) {
                break;
            }
        }
        int lCnt = i - il;
        
        TreeNode* root = new TreeNode(x);
        root->left = helper(preorder, pl + 1, pl + lCnt,
                            inorder, il, i - 1);
        root->right = helper(preorder, pl + lCnt + 1, pr, 
                             inorder, i + 1, ir);
        return root;
    }
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        return helper(preorder, 0, n - 1, inorder, 0, n - 1);
    }
};
```

2. 优化
用哈希表预处理inorder数组，而不是每次实时去找x的索引；
查找索引的时间复杂度由O(n)降低为O(1)；

3. Code(https://leetcode.com/submissions/detail/421209740/)
AC
时间复杂度：O(n) 
空间复杂度：O(h) // 最坏情况下：h = n
```C++
class Solution {
private:
    unordered_map<int, int> m;
    void init(vector<int>& inorder) {
        int n = inorder.size();
        for (int i = 0; i <= n - 1; i++) {
            m[inorder[i]] = i;
        }
    }
    TreeNode* helper(vector<int>& preorder, int pl, int pr,
                     vector<int>& inorder, int il, int ir) {
        if (pl > pr) {
            return NULL;
        } else if (pl == pr) {
            return new TreeNode(preorder[pl]);
        }
        
        int x = preorder[pl];
        int i = m[x];
        int lCnt = i - il;
        
        TreeNode* root = new TreeNode(x);
        root->left = helper(preorder, pl + 1, pl + lCnt,
                            inorder, il, i - 1);
        root->right = helper(preorder, pl + lCnt + 1, pr, 
                             inorder, i + 1, ir);
        return root;
    }
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        init(inorder);
        int n = preorder.size();
        return helper(preorder, 0, n - 1, inorder, 0, n - 1);
    }
};
```