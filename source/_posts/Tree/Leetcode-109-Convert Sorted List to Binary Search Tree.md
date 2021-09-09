---
title: Leetcode-109.Convert Sorted List to Binary Search Tree
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Binary Tree
- DFS
- Linked List
---

## 一、题意

### 0、题目链接
[109.Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

### 1、Description
【输入】
给定一个严格升序的单链表，头结点为head；
【输出】
根据该单链表，构造一棵高度平衡的BST；
【要求】
无；

### 2、Example
Input:[-10,-3,0,5,9]
Output:[0,-3,9,-10,NULL,5,NULL]

<!-- more -->

### 3、Corner Case
1. 若head为NULL，返回NULL；

## 二、题解

### 1、Solution-1：Recursive
1. 递归 + 二分构造；

2. 利用快慢指针法，找到链表中间结点，并截断；

3. Code(https://leetcode.com/submissions/detail/419424761/)
AC
时间复杂度：O(n * log n)； // T(n) = n + 2 * T(n/2) = O(n * log n)，类似快排
空间复杂度：O(log n)； 
```C++
class Solution {
private:
    TreeNode* helper(ListNode* head) {
        if (head == NULL) {
            return NULL;
        }
        if (head->next == NULL) {
            return new TreeNode(head->val, NULL, NULL);
        }
        
        ListNode* pre = NULL;
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            pre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        pre->next = NULL;
        
        TreeNode* mid = new TreeNode(slow->val, NULL, NULL);
        mid->left = helper(head);
        mid->right = helper(slow->next);
        return mid;
    }
public:
    TreeNode* sortedListToBST(ListNode* head) {
        return helper(head);
    }
};
```

### 2、Solution-2：Recursive
1. 递归 + 二分构造；

2. 利用快慢指针法，找到链表中间结点，不截断；

3. Code(https://leetcode.com/submissions/detail/419427950/)
AC
时间复杂度：O(n * log n)； // T(n) = n + 2 * T(n/2) = O(n * log n)，类似快排
空间复杂度：O(log n)；  
```C++
class Solution {
private:
    TreeNode* helper(ListNode* head, ListNode* tail) {
        // 处理区间[head, tail)内的结点；
        if (head == tail) {
            return NULL;
        }
        
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != tail && fast->next != tail) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        TreeNode* mid = new TreeNode(slow->val, NULL, NULL);
        mid->left = helper(head, slow);
        mid->right = helper(slow->next, tail);
        return mid;
    }
public:
    TreeNode* sortedListToBST(ListNode* head) {
        return helper(head, NULL);
    }
};
```