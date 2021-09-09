---
title: Leetcode-114.Flatten Binary Tree to Linked List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
---

## 一、题意

### 0、题目链接
[114.Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
将二叉树扁平化，退化成一个单链表；
单链表的顺序：根、左、右；
【要求】
就地操作：在给定的二叉树上改变指针指向，不申请额外的空间存储结点；

### 2、Example
Input:  [1,2,3]
Output: [1,NULL,2,NULL,3]

<!-- more -->

### 3、Corner Case
1. 若root为NULL，无需处理，直接return；

## 二、题解

### 1、Solution-1：Recursive(DFS)
1. 遍历顺序：DFS的顺序（postorder：右、左、根）遍历二叉树中的所有结点；

2. head：已经flatten好的链表头结点；

3. Code(https://leetcode.com/submissions/detail/416629954/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
private:
    TreeNode* head = NULL;
public:
    void flatten(TreeNode* root) {
        if (root == NULL) {
            return;
        }
        flatten(root->right);
        flatten(root->left);
        root->right = head;
        root->left = NULL;
        head = root;
    }
};
```

### 2、Solution-2：Iterative
1. 由于答案顺序为：根 -> 左 -> 右
所以将右子树(cur->right)拼接到左子树(cur->left)的“最后一个”节点即可。
对于cur->left来说，“最后一个”节点即：cur->left子树中，第一个无右儿子的结点；

2. cur：用于遍历二叉树；
若cur无左子树：直接遍历到cur->right；
若cur有左子树：找到last；last后接cur->right；修改cur的left、right指针；遍历到cur->right；

3. Code(https://leetcode.com/submissions/detail/416635212/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode* cur = root;
        while (cur != NULL) {
            if (cur->left != NULL) {
                TreeNode* last = cur->left;
                while (last->right != NULL) {
                    last = last->right;
                }
                last->right = cur->right;
                cur->right = cur->left;
                cur->left = NULL;
            }
            cur = cur->right;
        }
    }
};
```