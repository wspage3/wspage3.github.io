---
title: Leetcode-145.Binary Tree Postorder Traversal
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Stack
---

## 一、题意

### 0、题目链接
[145.Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

### 1、Description
【输入】
给定一棵二叉树，根结点为root；
【输出】
对二叉树进行后序遍历：输出每个结点的val；以一维数组的形式返回；
【要求】
递归、迭代；

### 2、Example
Input: [1,null,2,3]
Output: [3,2,1]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：Recursive
1. 后序遍历：左儿子、右儿子、root

2. 对于当前结点来说：
递归调用左子树；
递归调用右子树；
输出自身val；

3. Code(https://leetcode.com/submissions/detail/399020445/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    void helper(TreeNode* root, vector<int>& ans) {
        if (root == NULL) {
            return;
        }
        helper(root->left, ans);
        helper(root->right, ans);
        ans.push_back(root->val);
    }
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        helper(root, ans);
        return ans;
    }
};
```

### 2、Solution-2：Iterative
1. 转换：
preorder：根、左、右
temp：根、右、左
postorder：左、右、根

2. 初始化：将root压入栈；

3. 每一次：
从栈顶取出一个结点cur；
输出其val；
将cur出栈；
将其左儿子（非空）入栈；
将其右儿子（非空）入栈；

4. 将ans逆序；

5. Code(https://leetcode.com/submissions/detail/399023065/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        if (root == NULL) {
            return ans;
        }
        
        stack<TreeNode*> s;
        s.push(root);
        
        while (!s.empty()) {
            TreeNode* cur = s.top();
            ans.push_back(cur->val);
            s.pop();
            if (cur->left != NULL) {
                s.push(cur->left);
            }
            if (cur->right != NULL) {
                s.push(cur->right);
            }
        }
        
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

### 3、Solution-3：Morris Traversal