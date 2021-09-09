---
title: Leetcode-637.Average of Levels in Binary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
---

## 一、题意

### 0、题目链接
[637.Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
对二叉树进行层次遍历：输出每一层的平均值；以一维数组的形式返回；
【要求】
无；

### 2、Example
Input: [3,9,20,null,null,15,7]
Output: [3,14.5,11]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：BFS Using Queue
1. 执行正常的层次遍历；

2. 对于每一层，不用将所有结点的val存储在一维数组level中，
而是：将所有结点的val累加到sum中，sum最后再除以n；

3. Code(https://leetcode.com/submissions/detail/399462091/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> ans;
        if (root == NULL) {
            return ans;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            double sum = 0;
            int n = q.size();
            for (int i = 1; i <= n; i++) {
                TreeNode* node = q.front();
                q.pop();
                sum += node->val;
                if (node->left != NULL) {
                    q.push(node->left);
                }
                if (node->right != NULL) {
                    q.push(node->right);
                }
            }
            ans.push_back(sum / n);
        }
        
        return ans;
    }
};
```