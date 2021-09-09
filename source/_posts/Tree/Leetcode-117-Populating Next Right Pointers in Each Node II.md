---
title: Leetcode-117.Populating Next Right Pointers in Each Node II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- BFS
---

## 一、题意

### 0、题目链接
[117.Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

### 1、Description
【输入】
给定一棵Binary Tree，根结点为root；
除此之外，每个结点有一个next指针，初始化均为NULL；
【输出】
对树中所有结点的next指针域赋值：指向其右侧结点，若没有右侧结点，指向NULL即可；
【要求】
空间复杂度为O(1)，本题不考虑递归栈的开销；

<!-- more -->

### 2、Corner Case
1. 若root == NULL，无需处理；
2. 若root为单结点，无需处理；

## 二、题解

### 1、Solution-1：Iterative(BFS)
1. 假设树高为h，高度为：[0, h - 1]；

2. 外层循环进行竖直方向的遍历，遍历h层；

3. 内层循环进行水平方向的遍历；

4. 遍历到某一层的时候：
本层所有结点的next域已经处理完成；
考虑本层所有结点的子结点的next域，将其依次串联成一个单链表即可；
本层所有结点遍历结束后，跳到上述单链表的头结点，即下一层的最左边结点；

5. Code(https://leetcode.com/submissions/detail/414039631/)
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
        // 二叉树有h层[0, h -1]，则遍历h次；
        Node* curLevel = root;
        while (curLevel != NULL) {
            Node dummy(-1, NULL, NULL, NULL);
            Node* curTail = &dummy;
            // curNode：水平方向遍历；
            Node* curNode = curLevel;
            while (curNode != NULL) {
                if (curNode->left != NULL) {
                    curTail->next = curNode->left;
                    curTail = curTail->next;
                }
                if (curNode->right != NULL) {
                    curTail->next = curNode->right;
                    curTail = curTail->next;
                }
                curNode = curNode->next;
            }
            curLevel = dummy.next;
        }
        
        return root;
    }
};
```