---
title: Leetcode-589.N-ary Tree Preorder Traversal
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Tree
- Stack
---

## 一、题意

### 0、题目链接
[589.N-ary Tree Preorder Traversal](https://leetcode.com/problems/n-ary-tree-preorder-traversal/)

### 1、Description
【输入】
给定一棵N叉树，根结点为root；
【输出】
对N叉树进行前序遍历：输出每个结点的val；以一维数组的形式返回；
【要求】
递归、迭代；

### 2、Example
Input: root = [1,3,2,4]
Output: [1,3,2,4]

<!-- more -->

### 3、Corner Case
1. 若root == NULL，输出空数组；

## 二、题解

### 1、Solution-1：Recursive
1. 前序遍历：root、左儿子、...、右儿子

2. 对于当前结点来说：
输出自身val；
递归调用左子树；
递归调用...；
递归调用右子树；

3. Code(https://leetcode.com/submissions/detail/398842387/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
private:
    void helper(Node* root, vector<int>& ans) {
        if (root == NULL) {
            return;
        }
        ans.push_back(root->val);
        for (Node* child : root->children) {
            helper(child, ans);
        }
    }
public:
    vector<int> preorder(Node* root) {
        vector<int> ans;
        helper(root, ans);
        return ans;
    }
};
```

### 2、Solution-2：Iterative
1. 使用栈来模拟；

2. 初始化：将root压入栈；

3. 每一次：
从栈顶取出一个结点cur；
输出其val；
将cur出栈；
将其右儿子（非空）入栈；
将其...（非空）入栈；
将其左儿子（非空）入栈；

4. Code(https://leetcode.com/submissions/detail/398845763/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> ans;
        if (root == NULL) {
            return ans;
        }
        
        stack<Node*> s;
        s.push(root);
        
        while (!s.empty()) {
            Node* cur = s.top();
            ans.push_back(cur->val);
            s.pop();
            int n = cur->children.size();
            for (int i = n - 1; i >= 0; i--) {
                if (cur->children[i] != NULL) {
                    s.push(cur->children[i]);
                }
            }
        }
        
        return ans;
    }
};
```