---
title: Leetcode-173.Binary Search Tree Iterator
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Stack
- DFS
- Design
---

## 一、题意

### 0、题目链接
[173.Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

### 1、Description
【输入】
给定一棵二叉搜索树，根结点为root；
实现一个类BSTIterator：
构造函数：用root来初始化；
next：找出BST中下一个最小的值；
hasNext：判断BST中是否存在下一个最小的值；
【输出】
如上；
【要求】
O(1)的平均时间复杂度，O(h)的空间复杂度；

### 2、Example
[7,3,15,null,null,9,20]
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false

<!-- more -->

### 3、Corner Case
1. next()调用保证合法：当调用时，一定存在答案；

## 二、题解

### 1、Solution-1：Iterative(Inorder)
1. 迭代形式的中序遍历；

2. Code(https://leetcode.com/submissions/detail/419111756/)
AC
时间复杂度：初始化-O(h)，next-O(1)，hasNext-O(1)； 
空间复杂度：初始化-O(h)，next-O(h)，hasNext-O(1)； 
```C++
class BSTIterator {
private:
    stack<TreeNode*> s;
public:
    BSTIterator(TreeNode* root) {
        TreeNode* cur = root;
        while (cur != NULL) {
            s.push(cur);
            cur = cur->left;
        }
    }
    
    /** @return the next smallest number */
    int next() {
        TreeNode* node = s.top();
        s.pop();
        TreeNode* cur = node->right;
        while (cur != NULL) {
            s.push(cur);
            cur = cur->left;
        }
        return node->val;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !s.empty();
    }
};
```