---
title: Leetcode-543.Diameter of Binary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
---

## 一、题意

### 0、题目链接
[543.Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

### 1、Description
【输入】
给定一个二叉树，根结点为root；
【输出】
求出该二叉树的“直径”；
“直径”：树中任意两个节点构成的路径，最长的那条路径上边的数目；
【要求】
无；

### 2、Example
Input: [1,2,3,4,5,NULL,NULL]
Output: 3
Explanation: Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

<!-- more -->

### 3、Corner Case
1. 树为空时，return 0；
2. 树为单节点时，return 0；
3. 考虑结点数目 >= 2的情况；

## 二、题解

### 0、思考
1. 在二叉树中找到一条路径，其长度最大。

2. 这条路径一定会经过若干个结点，假设最上方（靠近root）的那个结点为node。
则对于node来说，只要计算出：其左子树的高度l、右子树的高度r，就可以算出经过node的路径长度：l + r + 2。

3. 参考(https://www.cnblogs.com/miloyip/archive/2010/02/25/binary_tree_distance.html)

### 1、Solution-1：DFS
1. 使用maxLen记录最大的“直径”，初始化为0；

2. 从root开始，遍历所有的结点。

3. 遍历到结点node时，node需要做以下两件事情：
第一，询问我的左儿子、右儿子，它们各自的“高度”是多少；在得知它们俩的“高度”后，更新maxLen；
第二，得知左儿子、右儿子的“高度”后，将我的高度返回给父节点，供其使用；

4. Code(https://leetcode.com/submissions/detail/337571902/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    int maxLen = 0;
    int helper(TreeNode* node) {
        if (node == NULL) {
            return -1;
        }
        int l = helper(node->left);
        int r = helper(node->right);
        maxLen = max(maxLen, l + r + 2);
        return max(l, r) + 1;
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        helper(root);
        return maxLen;
    }
};
```