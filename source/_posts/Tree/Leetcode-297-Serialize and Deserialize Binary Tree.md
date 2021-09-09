---
title: Leetcode-297.Serialize and Deserialize Binary Tree
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
[297.Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

### 1、Description
【输入】
设计二叉树的序列化与反序列化；
【输出】
序列化：给定一棵二叉树，根结点为root；输出一个字符串；
反序列化：给定一个字符串；输出二叉树的根节点；
【要求】
无；

### 2、Example
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]

<!-- more -->

### 3、Corner Case
1. 特判root为NULL的情况；

## 二、题解

### 1、Solution-1：BFS Using Queue
1. 序列化：采用level-order的顺序遍历二叉树；
迭代形式，用队列模拟；
例如：root = [1, 2, 3, null, null, 4, 5]
序列化后，得到的字符串ans为："1,2,3,n,n,4,5,n,n,n,n,"

2. 反序列化：
先将字符串ans转换为字符串数组v；v = [1, 2, 3, n, n, 4, 5, n, n, n, n]
同样，用队列模拟；
队列中的元素，其左右儿子需要被赋值；
每次取队首元素进行处理；

3. Code(https://leetcode.com/submissions/detail/420088361/)
AC
时间复杂度：序列化-O(n)，反序列化O(n)； 
空间复杂度：序列化-O(w)，反序列化O(w)；
```C++
class Codec {
private:
    // 两种方法，将字符串data转换为字符串数组v；
    void parse(string& data, vector<string>& v) {
        int len = data.size();
        int i = 0;
        while (i <= len - 1) {
            string s;
            while (i <= len - 1 && data[i] != ',') {
                s.push_back(data[i]);
                i++;
            }
            v.push_back(s);
            i++;
        }
    }
    void parse(string& data, vector<string>& v) {
        int len = data.size();
        int i = 0;
        while (i <= len - 1) {
            int j = data.find(',', i);
            v.push_back(data.substr(i, j - i));
            i = j + 1;
        }
    }
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string ans;
        if (root == NULL) {
            return ans;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* cur = q.front();
            q.pop();
            if (cur == NULL) {
                ans += "n,";
            } else {
                ans += to_string(cur->val) + ",";
                q.push(cur->left);
                q.push(cur->right);
            }
        }
        return ans;
    }
 
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int len = data.size();
        if (len == 0) {
            return NULL;
        }
        
        vector<string> v;
        parse(data, v);
        int n = v.size();
        
        TreeNode* root = new TreeNode(stoi(v[0]));
        queue<TreeNode*> q;
        q.push(root);
        for (int i = 1; i <= n - 1; ) {
            TreeNode* cur = q.front();
            q.pop();
            if (v[i] != "n") {
                TreeNode* l = new TreeNode(stoi(v[i]));
                cur->left = l;
                q.push(l);
            }
            i++;
            if (v[i] != "n") {
                TreeNode* r = new TreeNode(stoi(v[i]));
                cur->right = r;
                q.push(r);
            }
            i++;
        }
        
        return root;
    }
};
```

### 2、Solution-2：DFS Using Recursion
1. 序列化：采用preorder的顺序遍历二叉树；
例如：root = [1, 2, 3]
序列化后，得到的字符串ans为："1,2,n,n,3,n,n,"

2. 反序列化：
先将字符串ans转换为字符串数组v；v = [1, 2, n, n, 3, n, n]
然后根据传入的索引i，进行递归解决；

3. Code(https://leetcode.com/submissions/detail/420152977/)
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
            string s;
            int j = data.find(',', i);
            v.push_back(data.substr(i, j - i));
            i = j + 1;
        }
    }
    
    TreeNode* myDeserialize(vector<string>& v, int& i) {
        string cur = v[i];
        if (cur == "n") {
            i++;
            return NULL;
        }
        TreeNode* root = new TreeNode(stoi(cur));
        i++;
        root->left = myDeserialize(v, i);
        root->right = myDeserialize(v, i);
        return root;
    }
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string ans;
        if (root == NULL) {
            return "n,";
        }
        
        ans += to_string(root->val) + ",";
        ans += serialize(root->left);
        ans += serialize(root->right);
        
        return ans;
    }
 
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data == "n,") {
            return NULL;
        }
        
        vector<string> v;
        parse(data, v);
        int i = 0;
        return myDeserialize(v, i);
    }
};
```