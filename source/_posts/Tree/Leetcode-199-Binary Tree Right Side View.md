---
title: Leetcode-199.Binary Tree Right Side View
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
[199.Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
输出二叉树每一层的最右边的结点，以一维数组的形式返回；
【要求】
无；

### 2、Example
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：BFS Using Queue
1. Code(https://leetcode.com/submissions/detail/410970616/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        if (root == NULL) {
            return ans;
        }
        
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
                if (num == size) {
                    ans.push_back(cur->val);
                }
            }
        }
        
        return ans;
    }
};
```

### 2、Solution-2：DFS
1. 以DFS的顺序（根、右、左）访问树中所有的结点；

2. 访问到结点node时，函数参数会携带其所在层数level；
其中root对应的level为0；

3. Code(https://leetcode.com/submissions/detail/410978117/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    void helper(TreeNode* root, int level, vector<int>& ans) {
        if (root == NULL) {
            return;
        }
        
        int n = ans.size();
        if (n == level) {
            ans.push_back(root->val);
        }
        
        helper(root->right, level + 1, ans);
        helper(root->left, level + 1, ans);
    }
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        helper(root, 0, ans);
        return ans;
    }
};
```