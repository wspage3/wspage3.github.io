---
title: Leetcode-257.Binary Tree Paths
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
[257.Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
求出所有从根结点到叶结点的路径；
一条路径用一个字符串表示，最终输出一个一维字符串数组；
【要求】
无；

### 2、Example
Input:  [1,2,3]
Output: ["1->2","1->3"]

<!-- more -->

### 3、Corner Case
1. 若root为NULL，返回空一维数组；

## 二、题解

### 1、Solution-1：Backtracking(DFS)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. Code(https://leetcode.com/submissions/detail/416590590/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
private:
    void backtracking(TreeNode* node, string& s, vector<string>& ans) {
        if (node == NULL) {
            return;
        }
        if (s.size() > 0) {
            s += "->";
        }
        s += to_string(node->val);
        if (node->left == NULL && node->right == NULL) {
            ans.push_back(s);
        }
        backtracking(node->left, s, ans);
        backtracking(node->right, s, ans);
        while (s.size() > 0 && s.back() != '>') {
            s.pop_back();
        }
        if (s.size() > 0) {
            s.pop_back();
            s.pop_back();
        }
    }
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> ans;
        string s;
        backtracking(root, s, ans);
        return ans;
    }
};
```

### 2、Solution-2：DFS(Iterative, using stack)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. 辅助栈：栈中每个元素是一个字符串，代表一条路径；

3. 缺点：辅助栈消耗大量空间；

### 3、Solution-3：BFS(Iterative, using queue)
1. 遍历顺序：BFS的顺序（level-order）访问二叉树中的所有结点；

2. 辅助队列：队列中每个元素是一个字符串，代表一条路径；

3. 缺点：辅助队列消耗大量空间；