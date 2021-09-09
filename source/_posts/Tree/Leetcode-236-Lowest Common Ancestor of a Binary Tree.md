---
title: Leetcode-236.Lowest Common Ancestor of a Binary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
- BFS
- Queue
- Hash Table
---

## 一、题意

### 0、题目链接
[236.Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
给定上述二叉树中的两个结点p、q；
【输出】
求出p、q的LCA；
【要求】
无；

### 2、Example
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.

<!-- more -->

### 3、Corner Case
1. 2 <= n <= 100000
2. 二叉树中没有val值相同的结点；
3. p != q
4. p、q一定在上述BST中；

## 二、题解

### 1、Solution-1：DFS Using Recursion
1. 遍历方向：自顶向下的DFS

2. 遍历到结点node时，代表：在以node为根节点的二叉树中，搜索目标结点p和q；
首先，判断当前二叉树是否为空；
其次，判断当前根节点是否为p、q之一；
然后，询问左、右子树：“你们俩看到p、q了吗？”
最后，进行决策，向上一层汇报；

3. Code(https://leetcode.com/submissions/detail/421551075/)
AC
时间复杂度：O(n) 
空间复杂度：O(h) 
```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) {
            return NULL;
        }
        if (root == p || root == q) {
            return root;
        }
        
        TreeNode* l = lowestCommonAncestor(root->left, p, q);
        TreeNode* r = lowestCommonAncestor(root->right, p, q);
        if (l == NULL && r == NULL) {
            return NULL;
        } else if (l != NULL && r != NULL) {
            return root;
        } else {
            return l != NULL ? l : r;
        }
    }
};
```

### 2、Solution-2：Iterative
1. 从root开始，自顶向下的对二叉树进行BFS；

2. 记录每一个结点的父结点，存在哈希表m中；

3. 当p、q均被遍历到的时候，结束BFS；

4. 从p开始，到root为止（自下而上），访问这条路径上的每个结点，记录在us中；

5. 从q开始，到root为止（自下而上），当发现这条路径上的某个结点cur已经在us中，cur即二者的LCA；

6. Code(https://leetcode.com/submissions/detail/421586185/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        unordered_map<TreeNode*, TreeNode*> m;
        m[root] = NULL;
        queue<TreeNode*> myQueue;
        myQueue.push(root);
        while (m.count(p) == 0 || m.count(q) == 0) {
            TreeNode* node = myQueue.front();
            myQueue.pop();
            if (node->left != NULL) {
                m[node->left] = node;
                myQueue.push(node->left);
            }
            if (node->right != NULL) {
                m[node->right] = node;
                myQueue.push(node->right);
            }
        }
        
        unordered_set<TreeNode*> us;
        TreeNode* cur = p;
        while (cur != NULL) {
            us.insert(cur);
            cur = m[cur];
        }
        cur = q;
        while (cur != NULL && us.count(cur) == 0) {
            us.insert(cur);
            cur = m[cur];
        }
        return cur;
    }
};
```