---
title: Leetcode-429.N-ary Tree Level Order Traversal
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Tree
- Queue
---

## 一、题意

### 0、题目链接
[429.N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)

### 1、Description
【输入】
给定一棵N叉树，根结点为root；
【输出】
对N叉树进行层次遍历：输出每个结点的val；以二维数组的形式返回：每层对应一个一维数组；
【要求】
无；

### 2、Example
Input: [1,2,3,4] (N = 3)
Output: 
[
  [1],
  [2,3,4]
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
从左儿子到右儿子，将其所有非空儿子入队；

3. Code(https://leetcode.com/submissions/detail/399466445/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> ans;
        if (root == NULL) {
            return ans;
        }
        
        queue<Node*> q;
        q.push(root);
        
        while (!q.empty()) {
            vector<int> level;
            int n = q.size();
            for (int i = 1; i <= n; i++) {
                Node* node = q.front();
                q.pop();
                level.push_back(node->val);
                for (Node* child : node->children) {
                    if (child != NULL) {
                        q.push(child);
                    }
                }
            }
            ans.push_back(level);
        }
        
        return ans;
    }
};
```