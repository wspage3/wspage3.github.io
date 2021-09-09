---
title: Leetcode-106.Construct Binary Tree from Inorder and Postorder Traversal
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
[106.Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### 1、Description
【输入】
给定两个整数数组inorder、postorder，长度均为n；
分别代表二叉树的中序、后序遍历序列；
每个数组中均没有重复元素，即二叉树中没有相同val的结点；
【输出】
构建上述二叉树，返回根结点；
【要求】
无；

### 2、Example
Input: 
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]

<!-- more -->

### 3、Corner Case
1. 特判二叉树为NULL的情况；

## 二、题解

### 1、Solution-1：DFS Using Recursion
1. Code(https://leetcode.com/submissions/detail/420824116/)
AC
时间复杂度：O(n * n) // 最坏情况下：二叉树没有右儿子，每次查找x的索引均为O(n)；
空间复杂度：O(h) // 最坏情况下：h = n
```C++
class Solution {
private:
    TreeNode* helper(vector<int>& inorder, int il, int ir,
                     vector<int>& postorder, int pl, int pr) {
        if (il > ir) {
            return NULL;
        } else if (il == ir) {
            return new TreeNode(inorder[il]);
        }
        
        int x = postorder[pr];
        int i;
        for (i = il; i <= ir; i++) {
            if (inorder[i] == x) {
                break;
            }
        }
        int lCnt = i - il;
        
        TreeNode* root = new TreeNode(x);
        root->left = helper(inorder, il, i - 1,
                            postorder, pl, pl + lCnt - 1);
        root->right = helper(inorder, i + 1, ir,
                             postorder, pl + lCnt, pr - 1);
        return root;
    }
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int n = inorder.size();
        return helper(inorder, 0, n - 1, 
                      postorder, 0, n - 1);
    }
};
```

2. 优化
用哈希表预处理inorder数组，而不是每次实时去找x的索引；
查找索引的时间复杂度由O(n)降低为O(1)；

3. Code(https://leetcode.com/submissions/detail/421211972/)
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
    TreeNode* helper(vector<int>& inorder, int il, int ir,
                     vector<int>& postorder, int pl, int pr) {
        if (il > ir) {
            return NULL;
        } else if (il == ir) {
            return new TreeNode(inorder[il]);
        }
        
        int x = postorder[pr];
        int i = m[x];
        int lCnt = i - il;
        
        TreeNode* root = new TreeNode(x);
        root->left = helper(inorder, il, i - 1,
                            postorder, pl, pl + lCnt - 1);
        root->right = helper(inorder, i + 1, ir,
                             postorder, pl + lCnt, pr - 1);
        return root;
    }
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        init(inorder);
        int n = inorder.size();
        return helper(inorder, 0, n - 1, 
                      postorder, 0, n - 1);
    }
};
```