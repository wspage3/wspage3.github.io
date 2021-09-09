---
title: Leetcode-100.Same Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
- DFS
- BFS
---

## 一、题意

### 0、题目链接
[100.Same Tree](https://leetcode.com/problems/same-tree/)

### 1、Description
【输入】
给定两棵二叉树，根结点为p、q；
【输出】
判断这两棵二叉树是否相同，即树结构完全相同，每个结点的值也完全相同；
【要求】
无；

### 2、Example
Input:  [1,2,3], [1,2,3]
Output: true

<!-- more -->

### 3、Corner Case
1. 若p、q均为NULL，输出true；
2. 若p、q只有一个为NULL，输出false；
3. 考虑p、q均不为NULL的一般情况；

## 二、题解

### 1、Solution-1：DFS
1. 以DFS的顺序（preorder）访问树中所有的结点；

2. 对于每一对结点curP、curQ来说，只要保证：
二者的值相等；
二者的左子树相同；
二者的右子树相同；
则：以curP、curQ为根的两棵树是相同的；

3. Code(https://leetcode.com/submissions/detail/414735008/)
AC
时间复杂度：O(n)
空间复杂度：O(h)
```C++
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == NULL || q == NULL) {
            return p == NULL && q == NULL ? true : false;
        }
        if (p->val != q->val) {
            return false;
        }
        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
};
```

### 2、Solution-2：BFS Using Queue
1. 对p、q两棵树进行层次遍历：
依次对比二者的每一个结点；

2. Code(https://leetcode.com/submissions/detail/414738076/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == NULL || q == NULL) {
            return p == NULL && q == NULL ? true : false;
        }
        
        queue<TreeNode*> queueP;
        queue<TreeNode*> queueQ;
        queueP.push(p);
        queueQ.push(q);
        while (!queueP.empty() && !queueQ.empty()) {
            TreeNode* curP = queueP.front();
            TreeNode* curQ = queueQ.front();
            queueP.pop();
            queueQ.pop();
            if (curP == NULL && curQ == NULL) {
                continue;
            } else if (curP == NULL || curQ == NULL) {
                return false;
            } else {
                if (curP->val != curQ->val) {
                    return false;
                }
                queueP.push(curP->left);
                queueP.push(curP->right);
                queueQ.push(curQ->left);
                queueQ.push(curQ->right);
            }
        }
        
        if (queueP.empty() && queueQ.empty()) {
            return true;
        }
        
        return false;
    }
};
```