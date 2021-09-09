---
title: Leetcode-082.Remove Duplicates from Sorted List II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[082.Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
单链表是有序的（非降序）；
【输出】
将单链表中的重复元素去除，使得继续保持有序的情况下：
原先出现一次的元素，继续出现一次；
原先出现若干次的元素，将其全部删除；
【要求】
无；

### 2、Example
Input: 1->2->3->3->4->4->5
Output: 1->2->5

<!-- more -->

### 3、Corner Case
1. 若head为NULL，直接返回head；
2. 若head为单结点，直接返回head；

## 二、题解

### 1、Solution-1：Two Pointers without Free Memory
1. 思路：slow指向某一批的第一个结点，fast指向下一批的第一个结点；

2. 若fast与slow之间的距离大于1，说明slow这一批出现了不止一次，跳过；

3. Code(https://leetcode.com/submissions/detail/392614018/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        ListNode dummy(-1, NULL);
        ListNode* tail = &dummy;
        
        ListNode* slow = head;
        while (slow != NULL) {
            int distance = 1;
            ListNode* fast = slow->next;
            while (fast != NULL && fast->val == slow->val) {
                fast = fast->next;
                distance++;
            }
            
            if (distance == 1) {
                tail->next = slow;
                tail = tail->next;
                slow = fast;
            } else {
                slow = fast;
            }
        }
        tail->next = NULL;
        
        return dummy.next;
    }
};
```

### 2、Solution-2：Two Pointers with Free Memory
1. fast向后移动的同时，将其free；

2. 考虑是否需要free slow；

3. Code(https://leetcode.com/submissions/detail/392625179/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        ListNode dummy(-1, NULL);
        ListNode* tail = &dummy;
        
        ListNode* slow = head;
        while (slow != NULL) {
            int distance = 1;
            ListNode* fast = slow->next;
            while (fast != NULL && fast->val == slow->val) {
                ListNode* temp = fast; // free fast
                fast = fast->next;
                slow->next = fast; // free fast
                delete temp; // free fast
                distance++;
            }
            
            if (distance == 1) {
                tail->next = slow;
                tail = tail->next;
                slow = fast;
            } else {
                ListNode* temp = slow; // free slow
                slow = fast;
                delete temp; // free slow
            }
        }
        tail->next = NULL;
        
        return dummy.next;
    }
};
```