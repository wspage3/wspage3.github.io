---
title: Leetcode-129.Sum Root to Leaf Numbers
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
- Stack
- DFS
- BFS
---

## 一、题意

### 0、题目链接
[129.Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
所有结点的val值范围均在：[0, 9]内；
【输出】
每条从根节点到叶结点的路径，代表了一个值；例如1->2->5代表125；
求出二叉树中所有路径代表的值的和；
【要求】
无；

### 2、Example
Input:  [1,2,3]
Output: 25

<!-- more -->

### 3、Corner Case
1. 若root为NULL，输出0；

## 二、题解

### 1、Solution-1：DFS(Recursive)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. 辅助函数：int dfs(node, sum)
当调用时，表示：
当前准备对结点node进行遍历；
node的父节点代表的值为sum；

3. Code(https://leetcode.com/submissions/detail/416322182/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
    int dfs(TreeNode* node, int sum) {
        if (node == NULL) {
            return 0;
        }
        if (node->left == NULL && node->right == NULL) {
            return sum * 10 + node->val;
        }
        int l = dfs(node->left, sum * 10 + node->val);
        int r = dfs(node->right, sum * 10 + node->val);
        return l + r;
    }
public:
    int sumNumbers(TreeNode* root) {
        return dfs(root, 0);
    }
};
```

### 2、Solution-2：DFS(Iterative, using stack)
1. 遍历顺序：DFS的顺序（preorder）访问二叉树中的所有结点；

2. 用stack模拟perorder的遍历过程；
遍历到某个结点node时，vs辅助栈记录node代表的值；

3. 空间优化：不使用辅助栈vs，直接修改结点的val值；

4. Code(https://leetcode.com/submissions/detail/416324124/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        
        int sum = 0;
        stack<TreeNode*> s;
        s.push(root);
        stack<int> vs;
        vs.push(root->val);
        while (!s.empty()) {
            TreeNode* cur = s.top();
            s.pop();
            int curVal = vs.top();
            vs.pop();
            if (cur->left == NULL && cur->right == NULL) {
                sum += curVal;
            }
            if (cur->right != NULL) {
                s.push(cur->right);
                vs.push(curVal * 10 + cur->right->val);
            }
            if (cur->left != NULL) {
                s.push(cur->left);
                vs.push(curVal * 10 + cur->left->val);
            }
        }
        return sum;
    }
};
```

### 3、Solution-3：BFS(Iterative, using queue)
1. 遍历顺序：BFS的顺序（level-order）访问二叉树中的所有结点；

2. 用queue模拟level-order的遍历过程；
遍历到某个结点node时，vq辅助队列记录node代表的值；

3. 空间优化：不使用辅助栈vq，直接修改结点的val值；

4. Code(https://leetcode.com/submissions/detail/416324659/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        
        int sum = 0;
        queue<TreeNode*> q;
        q.push(root);
        queue<int> vq;
        vq.push(root->val);
        while (!q.empty()) {
            TreeNode* cur = q.front();
            q.pop();
            int curVal = vq.front();
            vq.pop();
            if (cur->left == NULL && cur->right == NULL) {
                sum += curVal;
            }
            if (cur->left != NULL) {
                q.push(cur->left);
                vq.push(curVal * 10 + cur->left->val);
            }
            if (cur->right != NULL) {
                q.push(cur->right);
                vq.push(curVal * 10 + cur->right->val);
            }
        }
        return sum;
    }
};
```