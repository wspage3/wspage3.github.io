---
title: Leetcode-116.Populating Next Right Pointers in Each Node
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- BFS
- DFS
---

## 一、题意

### 0、题目链接
[116.Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

### 1、Description
【输入】
给定一棵Perfect Binary Tree，根结点为root；
Perfect Binary Tree：所有叶结点在同一层；每个父结点有两个子结点；
除此之外，每个结点有一个next指针，初始化均为NULL；
【输出】
对树中所有结点的next指针域赋值：指向其右侧结点，若没有右侧结点，指向NULL即可；
【要求】
递归、迭代；
空间复杂度为O(1)，本题不考虑递归栈的开销；

<!-- more -->

### 2、Corner Case
1. 若root == NULL，无需处理；
2. 若root为单结点，无需处理；

## 二、题解

### 1、Solution-1：Iterative(BFS)
1. 假设树高为h，高度为：[0, h - 1]；

2. 外层循环进行竖直方向的遍历，遍历前h - 1层；

3. 内层循环进行水平方向的遍历；

4. Code(https://leetcode.com/submissions/detail/413986884/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    Node* connect(Node* root) {
        if (root == NULL) {
            return root;
        }
        
        // curLevel：竖直方向遍历；
        // 二叉树有h层[0, h -1]，则遍历h - 1次；
        Node* curLevel = root;
        while (curLevel != NULL && curLevel->left != NULL) {
            // curNode：水平方向遍历；
            Node* curNode = curLevel;
            while (curNode != NULL) {
                curNode->left->next = curNode->right;
                curNode->right->next = curNode->next == NULL ? NULL : curNode->next->left;
                curNode = curNode->next;
            }
            curLevel = curLevel->left;
        }
        
        return root;
    }
};
```

### 2、Solution-2：Recursive(DFS)
1. 采用preorder的顺序遍历树；

2. 对于结点root来说：处理其左儿子的next域、处理其右儿子的next域、递归调用；

3. Code(https://leetcode.com/submissions/detail/413990145/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
public:
    Node* connect(Node* root) {
        if (root == NULL) {
            return root;
        }
        
        if (root->left != NULL) {
            root->left->next = root->right;
            root->right->next = root->next == NULL ? NULL : root->next->left;
            connect(root->left);
            connect(root->right);
        }
        
        return root;
    }
};
```