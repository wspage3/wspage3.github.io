---
title: Leetcode-993.Cousins in Binary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Queue
- DFS
---

## 一、题意

### 0、题目链接
[993.Cousins in Binary Tree](https://leetcode.com/problems/cousins-in-binary-tree/)

### 1、Description
【输入】
给定一个二叉树，根结点为root；二叉树中每个结点的value值都不重复；
给定两个整数：x、y；分别代表二叉树中的两个结点；
【输出】
判断这两个结点是否是“表兄弟”；
“表兄弟”：深度相同，父节点不同；
“亲兄弟”：深度相同，父节点相同；
【要求】
无；

### 2、Example
Input: root = [1,2,3,4], x = 4, y = 3
Output: false

<!-- more -->

### 3、Corner Case
1. 2 <= n <= 100；
2. 二叉树中每个结点的value值都不重复；

## 二、题解

### 1、Solution-1：BFS Using Queue
1. 对二叉树进行层次遍历，可以判断给定的两个结点是否在同一层；

2. 同时需要判断，两个结点的父节点是否相同；

3. Code(https://leetcode.com/submissions/detail/410611746/)
AC
时间复杂度：O(n) 
空间复杂度：O(n) 
```C++
class Solution {
public:
    bool isCousins(TreeNode* root, int x, int y) {
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            int cnt = 0;
            int size = q.size();
            for (int num = 1; num <= size; num++) {
                TreeNode* cur = q.front();
                q.pop();
                if (cur->val == x || cur->val == y) {
                    cnt++;
                }
                if (cur->left != NULL && cur->right != NULL) {
                    bool flag1 = cur->left->val == x && cur->right->val == y;
                    bool flag2 = cur->left->val == y && cur->right->val == x;
                    if (flag1 || flag2) {
                        return false;
                    }
                    q.push(cur->left);
                    q.push(cur->right);
                } else if (cur->left != NULL) {
                    q.push(cur->left);
                } else if (cur->right != NULL) {
                    q.push(cur->right);
                }
            }
            if (cnt == 1) {
                return false;
            }
            if (cnt == 2) {
                return true;
            }
        }
        
        return false; // 如果x、y均未出现，返回false；
    }
};
```

### 2、Solution-2：DFS
1. 从root开始遍历：找出x、y各自的深度、以及其父节点；

2. 进行判断；

3. 递归到结点node时，深度为level。
判断左儿子是否为目标结点；
判断右儿子是否为目标结点；
如果都不是，递归调用其左右儿子；

4. Code(https://leetcode.com/submissions/detail/342524716/)
AC
时间复杂度：O(n) 
空间复杂度：O(h) // h最坏情况下为n，即二叉树退化为链表，需要递归n层；
```C++
class Solution {
private:
    void dfs(TreeNode* node, int level, int value, int& depth, int& father) {
        if (node == NULL) {
            return;            
        }
        if (node->left != NULL && node->left->val == value) {
            depth = level + 1;
            father = node->val;
            return;
        }
        if (node->right != NULL && node->right->val == value) {
            depth = level + 1;
            father = node->val;
            return;
        }
        dfs(node->left, level + 1, value, depth, father);
        dfs(node->right, level + 1, value, depth, father);
    }
public:
    bool isCousins(TreeNode* root, int x, int y) {
        int xDepth = -1;
        int yDepth = -1;
        int xFather = -1;
        int yFather = -1;
        dfs(root, 0, x, xDepth, xFather);
        dfs(root, 0, y, yDepth, yFather);
        return xDepth == yDepth && xFather != yFather;
    }
};
```