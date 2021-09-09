---
title: Leetcode-094.Binary Tree Inorder Traversal
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Stack
---

## 一、题意

### 0、题目链接
[094.Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
对二叉树进行中序遍历：输出每个结点的val；以一维数组的形式返回；
【要求】
递归、迭代；

### 2、Example
Input: [1,null,2,3]
Output: [1,3,2]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：Recursive
1. 中序遍历：左儿子、root、右儿子

2. 对于当前结点来说：
递归调用左子树；
输出自身val；
递归调用右子树；

3. Code(https://leetcode.com/submissions/detail/399106010/)
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
        helper(root->left, ans);
        ans.push_back(root->val);
        helper(root->right, ans);
    }
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        helper(root, ans);
        return ans;
    }
};
```

### 2、Solution-2：Iterative
1. 使用栈来模拟；

2. cur指针用于结点的遍历：
从root出发，往左下角的方向遍历，直到当前结点node没有左子树为止；
同时，将路径上的结点（包含node）都压入栈中保存；

3. 此时，node有两种情况：
node还有右子树：访问node（栈顶）；将node出栈；cur指向node->right，重复上述过程；
node没有右子树：访问node（栈顶）；将node出栈；若栈空，结束；若栈非空，回溯到node的父节点（栈顶）；

5. Code(https://leetcode.com/submissions/detail/399209165/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        if (root == NULL) {
            return ans;
        }
        
        TreeNode* cur = root;
        stack<TreeNode*> s;
        
        while (cur != NULL || !s.empty()) {
            while (cur != NULL) {
                s.push(cur);
                cur = cur->left;
            }
            TreeNode* node = s.top();
            ans.push_back(node->val);
            s.pop();
            cur = node->right;
        }
        
        return ans;
    }
};
```

### 3、Solution-3：Morris Traversal