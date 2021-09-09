---
title: Leetcode-103.Binary Tree Zigzag Level Order Traversal
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
---

## 一、题意

### 0、题目链接
[103.Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
对二叉树进行层次遍历：输出每个结点的val；以二维数组的形式返回：每层对应一个一维数组；
输出第一、三、五...层，正序（从左到右）输出；
输出第二、四、六...层，逆序（从右到左）输出；
【要求】
无；

### 2、Example
Input: [3,9,20,null,null,15,7]
Output: 
[
  [3],
  [20,9],
  [15,7]
]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：BFS Using Queue
1. 执行正常的层次遍历；

2. isReverse表示当前level是正序输出还是逆序输出；

3. Code(https://leetcode.com/submissions/detail/399945393/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (root == NULL) {
            return ans;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        bool isReverse = false;
        
        while (!q.empty()) {
            int n = q.size();
            vector<int> level(n);
            for (int i = 0; i <= n - 1; i++) {
                TreeNode* node = q.front();
                q.pop();
                if (isReverse) {
                    level[n - 1 - i] = node->val;
                } else {
                    level[i] = node->val;
                }
                if (node->left != NULL) {
                    q.push(node->left);
                }
                if (node->right != NULL) {
                    q.push(node->right);
                }
            }
            ans.push_back(level);
            isReverse = !isReverse;
        }
        
        return ans;
    }
};
```

### 2、Solution-2：BFS Using Deque
1. 正序：读取front；从front出队；从back入队儿子：先左后右；
逆序：读取back；从back出队；从front入队儿子：先右后左；

2. Code(https://leetcode.com/submissions/detail/399950199/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (root == NULL) {
            return ans;
        }
        
        deque<TreeNode*> dq;
        dq.push_back(root);
        bool isReverse = false;
        
        while (!dq.empty()) {
            int n = dq.size();
            vector<int> level;
            for (int i = 0; i <= n - 1; i++) {
                if (isReverse) {
                    TreeNode* node = dq.back();
                    dq.pop_back();
                    level.push_back(node->val);
                    if (node->right != NULL) {
                        dq.push_front(node->right);
                    }
                    if (node->left != NULL) {
                        dq.push_front(node->left);
                    }
                } else {
                    TreeNode* node = dq.front();
                    dq.pop_front();
                    level.push_back(node->val);
                    if (node->left != NULL) {
                        dq.push_back(node->left);
                    }
                    if (node->right != NULL) {
                        dq.push_back(node->right);
                    }
                }
            }
            ans.push_back(level);
            isReverse = !isReverse;
        }
        
        return ans;
    }
};
```