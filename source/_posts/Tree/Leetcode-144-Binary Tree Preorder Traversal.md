---
title: Leetcode-144.Binary Tree Preorder Traversal
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Stack
---

## 一、题意

### 0、题目链接
[144.Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
对二叉树进行前序遍历：输出每个结点的val；以一维数组的形式返回；
【要求】
递归、迭代；

### 2、Example
Input: [1,null,2,3]
Output: [1,2,3]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：Recursive
1. 前序遍历：root、左儿子、右儿子

2. 对于当前结点来说：
输出自身val；
递归调用左子树；
递归调用右子树；

3. Code(https://leetcode.com/submissions/detail/398672322/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    void helper(TreeNode* root, vector<int>& ans) {
        if (root == NULL) {
            return;
        }
        ans.push_back(root->val);
        helper(root->left, ans);
        helper(root->right, ans);
    }
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        helper(root, ans);
        return ans;
    }
};
```

### 2、Solution-2：Iterative
1. 使用栈来模拟；

2. 初始化：将root压入栈；

3. 每一次：
从栈顶取出一个结点cur；
输出其val；
将cur出栈；
将其右儿子（非空）入栈；
将其左儿子（非空）入栈；

4. Code(https://leetcode.com/submissions/detail/398679255/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        if (root == NULL) {
            return ans;
        }
        
        stack<TreeNode*> s;
        s.push(root);
        
        while (!s.empty()) {
            TreeNode* cur = s.top();
            ans.push_back(cur->val);
            s.pop();
            if (cur->right != NULL) {
                s.push(cur->right);
            }
            if (cur->left != NULL) {
                s.push(cur->left);
            }
        }
        
        return ans;
    }
};
```

### 3、Solution-3：Morris Traversal