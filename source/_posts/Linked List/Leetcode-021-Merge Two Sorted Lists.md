---
title: Leetcode-021.Merge Two Sorted Lists
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[021.Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

### 1、Description
【输入】
给定两个单链表，头结点分别为l1、l2；
两个单链表都是有序的（非降序）；
【输出】
将这两个单链表合并成一个单链表，同时保持有序的特点，返回头结点；
【要求】
不开辟额外的空间，新链表的结点就是给定两个单链表的所有结点；

### 2、Example
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4

<!-- more -->

### 3、Corner Case
1. 若l1为NULL，直接返回l2；
2. 若l2为NULL，直接返回l1；
3. 考虑l1、l2均不为NULL的情况；

## 二、题解

### 1、Solution-1：Two Pointers
1. 使用虚拟头结点dummy，方便返回结果，tail初始化指向dummy；

2. 每次从l1、l2中挑选一个较小的，接在tail的后面；

3. 当一个链表遍历结束的时候，将另一个链表接在tail的后面；

4. Code(https://leetcode.com/submissions/detail/390780846/)
AC
时间复杂度：O(m + n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL || l2 == NULL) {
            return l1 == NULL ? l2 : l1;
        }
        
        ListNode dummy(-1, NULL);
        ListNode* tail = &dummy;
        while (l1 != NULL && l2 != NULL) {
            if (l1->val <= l2->val) {
                tail->next = l1;
                l1 = l1->next;
            } else {
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }
        
        tail->next = (l1 == NULL ? l2 : l1);
        
        return dummy.next;
    }
};
```