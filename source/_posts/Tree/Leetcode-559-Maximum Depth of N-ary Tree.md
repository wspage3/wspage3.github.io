---
title: Leetcode-559.Maximum Depth of N-ary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Tree
- Queue
- DFS
---

## 一、题意

### 0、题目链接
[559.Maximum Depth of N-ary Tree](https://leetcode.com/problems/maximum-depth-of-n-ary-tree/)

### 1、Description
【输入】
给定一棵N叉树，根结点为root；
【输出】
求N叉树的最大深度；
本题中，root的深度为1；
【要求】
无；

### 2、Example
Input: root = [1,null,3,2,4,null,5,6]
Output: 3

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出0；

## 二、题解

### 1、Solution-1：DFS Using Recursion
1. Code(https://leetcode.com/submissions/detail/410891410/)
AC
时间复杂度：O(n)
空间复杂度：O(h) // h最坏情况下为n；
```C++
class Solution {
public:
    int maxDepth(Node* root) {
        if (root == NULL) {
            return 0;
        }
        
        int ans = 0;
        for (Node* child : root->children) {
            ans = max(ans, maxDepth(child));
        }
        
        return ans + 1;
    }
};
```

### 2、Solution-2：BFS Using Queue
1. Code(https://leetcode.com/submissions/detail/410892372/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int maxDepth(Node* root) {
        if (root == NULL) {
            return 0;
        }
        
        queue<Node*> q;
        q.push(root);
        
        int ans = 0;
        while (!q.empty()) {
            ans++;
            int size = q.size();
            for (int num = 1; num <= size; num++) {
                Node* cur = q.front();
                q.pop();
                for (Node* child : cur->children) {
                    if (child != NULL) {
                        q.push(child);
                    }
                }
            }
        }
        
        return ans;
    }
};
```