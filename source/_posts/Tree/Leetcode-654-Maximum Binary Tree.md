---
title: Leetcode-654.Maximum Binary Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
- Array
- Monotonous Stack
---

## 一、题意

### 0、题目链接
[654.Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

### 1、Description
【输入】
给定一个无重复元素的数组nums；
【输出】
根据nums，构造一棵“最大二叉树”；
最大二叉树，定义如下：
二叉树的根是：nums中最大的元素，其下标记为idx；
二叉树的左子树是：以nums[0, idx - 1]构造的最大二叉树；
二叉树的右子树是：以nums[idx + 1, n - 1]构造的最大二叉树；
【要求】
无；

### 2、Example
Input:[2,3,1]
Output:[3,2,1]

<!-- more -->

### 3、Corner Case
1. 若nums为空，返回NULL；

## 二、题解

### 1、Solution-1：Recursive
1. 递归 + 二分构造；

2. 时间复杂度分析：
类似快排：对于规模为n的问题，首先需要进行O(n)的遍历，其次分别处理两个子问题；
平均情况：T(n) = O(n * log n)
最坏情况，数组nums已经有序：T(n) = n + T(n - 1) = O(n * n)

3. Code(https://leetcode.com/submissions/detail/419443504/)
AC
时间复杂度：O(n * n)； 
空间复杂度：O(n)； 
```C++
class Solution {
private:
    TreeNode* helper(vector<int>& nums, int left, int right) {
        if (left > right) {
            return NULL;
        } 
        if (left == right) {
            return new TreeNode(nums[left], NULL, NULL);
        }
        
        int maxIdx = left;
        for (int i = left + 1; i <= right; i++) {
            if (nums[i] > nums[maxIdx]) {
                maxIdx = i;
            }
        }
        
        TreeNode* mid = new TreeNode(nums[maxIdx], NULL, NULL);
        mid->left = helper(nums, left, maxIdx - 1);
        mid->right = helper(nums, maxIdx + 1, right);
        return mid;
    }
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        int n = nums.size();
        return helper(nums, 0, n - 1);
    }
};
```

### 2、Solution-2：Monotonous Stack
1. 用单调栈构造一棵笛卡尔树；

2. 思路：
单调递减栈，即栈顶元素最小；
遍历nums的每一个元素，每次创建相应的结点cur；
把栈中小于cur->val的元素依次抛出，更新cur的左儿子；
若栈非空，说明当前栈顶元素大于cur->val，更新栈顶元素的右儿子；
找到答案，即栈中最后一个元素；

3. 参考(https://leetcode.com/problems/maximum-binary-tree/discuss/106147/C%2B%2B-8-lines-O(n-log-n)-map-plus-stack-with-binary-search)

4. Code(https://leetcode.com/submissions/detail/419516552/)
AC
时间复杂度：O(n)； 
空间复杂度：O(n)； 
```C++
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        int n = nums.size();
        stack<TreeNode*> s;
        for (int i = 0; i <= n - 1; i++) {
            TreeNode* cur = new TreeNode(nums[i]);
            while (!s.empty() && nums[i] > s.top()->val) {
                cur->left = s.top();
                s.pop();
            }
            if (!s.empty()) {
                s.top()->right = cur;
            }
            s.push(cur);
        }
        while (s.size() > 1) {
            s.pop();
        }
        return s.top();
    }
};
```