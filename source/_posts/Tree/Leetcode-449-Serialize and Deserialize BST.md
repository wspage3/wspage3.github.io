---
title: Leetcode-449.Serialize and Deserialize BST
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Design
- Binary Tree
- DFS
- BFS
- Queue
---

## 一、题意

### 0、题目链接
[449.Serialize and Deserialize BST](https://leetcode.com/problems/serialize-and-deserialize-bst/)

### 1、Description
【输入】
设计二叉搜索树的序列化与反序列化；
【输出】
序列化：给定一棵二叉搜索树，根结点为root；输出一个字符串；
反序列化：给定一个字符串；输出二叉搜索树的根节点；
【要求】
使得序列化后的字符串尽可能的紧凑；

### 2、Example
Input: root = [2,1,3]
Output: [2,1,3]

<!-- more -->

### 3、Corner Case
1. 特判root为NULL的情况；

## 二、题解

### 0、思考
1. BST一定是一棵二叉树，所以可以直接使用#297-Serialize and Deserialize Binary Tree的代码解决；

2. 题目要求使得序列化后的字符串尽可能的紧凑，可以结合BST的性质进行优化（NULL结点不出现在字符串中了）；

3. #297中，我们遇到NULL结点时，向递归树的上一层返回；

4. 本题中，当发现超过了取值范围后，向递归树的上一层返回；

### 1、Solution-1：DFS Using Recursion
1. 序列化：采用preorder的顺序遍历二叉树；
例如：root = [1, 2, 3]
序列化后，得到的字符串ans为："1,2,3,"

2. 反序列化：
先将字符串ans转换为字符串数组v；v = [1, 2, 3]
然后根据传入的索引i，以及期望儿子所在的取值范围，进行递归解决；

3. Code(https://leetcode.com/submissions/detail/420791873/)
AC
时间复杂度：序列化-O(n)，反序列化O(n)； 
空间复杂度：序列化-O(h)，反序列化O(h)；
```C++
class Codec {
private:
    void parse(string& data, vector<string>& v) {
        int len = data.size();
        int i = 0;
        while (i <= len - 1) {
            int j = data.find(',', i);
            v.push_back(data.substr(i, j - i));
            i = j + 1;
        }
    }
    TreeNode* myDeserialize(vector<string>& v, int& i, long long l, long long r) {
        int n = v.size();
        if (i >= n) {
            return NULL;
        }
        
        int cur = stoi(v[i]);
        if (cur <= l || cur >= r) {
            return NULL;
        }
        
        TreeNode* root = new TreeNode(cur);
        i++;
        root->left = myDeserialize(v, i, l, cur);
        root->right = myDeserialize(v, i, cur, r);
        return root;
    }
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (root == NULL) {
            return "";
        }
        
        return to_string(root->val) + "," + serialize(root->left) + serialize(root->right);
    }
 
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data == "") {
            return NULL;
        }
        
        vector<string> v;
        parse(data, v);
        int i = 0;
        return myDeserialize(v, i, LLONG_MIN, LLONG_MAX);
    }
};
```