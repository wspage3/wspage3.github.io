---
title: Leetcode-113.Path Sum II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Backtracking
- DFS
---

## 一、题意

### 0、题目链接
[113.Path Sum II](https://leetcode.com/problems/path-sum-ii/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
给定一个整数sum；
【输出】
求出所有从根结点到叶结点的路径，路径上的所有结点的和恰好为给定的sum；
一条路径用一个一维数组表示，最终输出一个二维数组；
【要求】
无；

### 2、Example
Input:  [1,2,3], sum = 3
Output: [[1,2]]

<!-- more -->

### 3、Corner Case
1. 若root为NULL，无论sum是多少，都不存在，返回空二维数组；

## 二、题解

### 1、Solution-1：Backtracking(DFS)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. 访问到本结点时：
将本结点添加到路径中；
判断：当前路径是否满足要求；
向下递归左右子树：
将本结点从路径中删除；
向上回溯；

3. Code(https://leetcode.com/submissions/detail/416283122/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
private:
    void backtracking(TreeNode* node, int sum, vector<int>& path, vector<vector<int>>& ans) {
        if (node == NULL) {
            return;
        }
        path.push_back(node->val);
        if (node->left == NULL && node->right == NULL && sum == node->val) {
            ans.push_back(path);
        }
        backtracking(node->left, sum - node->val, path, ans);
        backtracking(node->right, sum - node->val, path, ans);
        path.pop_back();
    }
public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<vector<int>> ans;
        vector<int> path;
        backtracking(root, sum, path, ans);
        return ans;
    }
};
```

### 2、Solution-2：DFS(Iterative, using stack)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. 辅助栈：栈中每个元素是一个一维数组，代表一条路径；

3. 缺点：辅助栈消耗大量空间；

### 3、Solution-3：BFS(Iterative, using queue)
1. 遍历顺序：BFS的顺序（level-order）访问二叉树中的所有结点；

2. 辅助队列：队列中每个元素是一个一维数组，代表一条路径；

3. 缺点：辅助队列消耗大量空间；