---
title: Leetcode-501.Find Mode in Binary Search Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- Stack
- DFS
---

## 一、题意

### 0、题目链接
[501.Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/)

### 1、Description
【输入】
给定一棵二叉搜索树，根结点为root；
本题中，二叉搜索树满足：
左子树的所有结点均小于等于根结点；
右子树的所有结点均大于等于根结点；
【输出】
求出该二叉搜索树中所有的众数（出现次数最多），以一维数组的形式返回，顺序不限；
【要求】
不考虑递归导致的栈空间的情况下，O(1)的空间复杂度；

### 2、Example
Input: root = [1,null,2,2]
Output: [2]

<!-- more -->

### 3、Corner Case
1. 若root为NULL，返回空一维数组；

## 二、题解

### 1、Solution-1：Iterative(Inorder)
1. 迭代形式的中序遍历；

2. Code(https://leetcode.com/submissions/detail/418348232/)
AC
时间复杂度：O(n) 
空间复杂度：O(n) // 空间复杂度不满足题意的O(1)；
```C++
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
        vector<int> ans;
        if (root == NULL) {
            return ans;
        }
        
        stack<TreeNode*> s;
        TreeNode* cur = root;
        
        int cnt = 0;
        int maxCnt = 0;
        long long pre = LLONG_MIN;
        
        while (cur != NULL || !s.empty()) {
            while (cur != NULL) {
                s.push(cur);
                cur = cur->left;
            }
            TreeNode* node = s.top();
            s.pop();
            cur = node->right;
            // 数据统计
            if (node->val == pre) {
                cnt++;
            } else {
                cnt = 1;
            }
            if (cnt == maxCnt) {
                ans.push_back(node->val);
            } else if (cnt > maxCnt) {
                ans.clear();
                ans.push_back(node->val);
                maxCnt = cnt;
            }
            pre = node->val;
        }
        
        return ans;
    }
};
```

### 2、Solution-2：Recursive(Inorder)
1. 递归形式的中序遍历；

2. 最坏情况下，空间复杂度非最优：
如中序遍历的结果为：[1, 2, 3, 4, 5, ..., n - 1, n - 1]
ans = [n - 1]
但遍历过程中，ans数组的规模会达到O(n)级别；

3. Code(https://leetcode.com/submissions/detail/418359959/)
AC
时间复杂度：O(n) 
空间复杂度：O(n) // 最坏情况下，尽管ans数组的规模为O(1)，但中间过程可能会达到O(n)；
```C++
class Solution {
private:
    vector<int> ans;
    int freq = 0;
    int maxFreq = 0;
    long long pre = LLONG_MIN;
    void inorder(TreeNode* node) {
        if (node == NULL) {
            return;
        }
        inorder(node->left);
        if (node->val == pre) {
            freq++;
        } else {
            freq = 1;
        }
        if (freq == maxFreq) {
            ans.push_back(node->val);
        } else if (freq > maxFreq) {
            ans.clear();
            ans.push_back(node->val);
            maxFreq = freq;
        }
        pre = node->val;
        inorder(node->right);
    }
public:
    vector<int> findMode(TreeNode* root) {
        inorder(root);
        return ans;
    }
};
```

### 3、Solution-3：Recursive(Two-round Traversal)
1. 两轮中序遍历；

2. 第一轮：确定ans数组的大小，记为modeCnt；

3. 第二轮：若发现某个数满足条件（freq == maxFreq），将其添加到ans数组中；

4. Code(https://leetcode.com/submissions/detail/418372786/)
AC
时间复杂度：O(n) 
空间复杂度：O(1) // 不考虑递归栈空间的使用的情况下，空间复杂度为O(1)；
```C++
class Solution {
private:
    vector<int> ans;
    int freq = 0;
    int maxFreq = 0;
    long long pre = LLONG_MIN;
    int modeCnt = 0;
    
    void inorder(TreeNode* node) {
        if (node == NULL) {
            return;
        }
        inorder(node->left);
        if (node->val == pre) {
            freq++;
        } else {
            freq = 1;
        }
        if (freq == maxFreq) {
            // 第二次遍历会进入此if语句
            if (ans.size() != 0) {
                ans[modeCnt] = node->val;
            }
            modeCnt++;
        } else if (freq > maxFreq) {
            modeCnt = 1;
            maxFreq = freq;
        }
        pre = node->val;
        inorder(node->right);
    }
public:
    vector<int> findMode(TreeNode* root) {
        inorder(root);
        ans.resize(modeCnt);
        freq = 0;
        pre = LLONG_MIN;
        modeCnt = 0;
        inorder(root);
        return ans;
    }
};
```