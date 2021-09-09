---
title: Leetcode-102.Binary Tree Level Order Traversal
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
[102.Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
对二叉树进行层次遍历：输出每个结点的val；以二维数组的形式返回：每层对应一个一维数组；
【要求】
递归、迭代；

### 2、Example
Input: [3,9,20,null,null,15,7]
Output: 
[
  [3],
  [9,20],
  [15,7]
]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：BFS Using Queue
1. 初始化：将root入队；

2. 每一轮，考察当前队列中所有结点（n个）：
访问val；
将其出队；
将其左（非空）、右（非空）儿子入队；

3. Code(https://leetcode.com/submissions/detail/399450036/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
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
        
        return ans;
    }
};
```

### 2、Solution-2：DFS
1. 以DFS的顺序（preorder）访问树中所有的结点；

2. 访问到结点node时，函数参数会携带其所在层数level；
其中root对应的level为0；

3. Code(https://leetcode.com/submissions/detail/399454937/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    void helper(TreeNode* root, int level, vector<vector<int>>& ans) {
        if (root == NULL) {
            return;
        }
        
        if (ans.size() == level) {
            vector<int> v;
            ans.push_back(v);
        }
        ans[level].push_back(root->val);
        
        helper(root->left, level + 1, ans);
        helper(root->right, level + 1, ans);
    }
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        helper(root, 0, ans);
        return ans;
    }
};
```