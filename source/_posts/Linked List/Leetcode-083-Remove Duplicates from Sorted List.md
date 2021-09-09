---
title: Leetcode-083.Remove Duplicates from Sorted List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[083.Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
单链表是有序的（非降序）；
【输出】
将单链表中的重复元素去除，使得继续保持有序的情况下，每个元素出现一次；
【要求】
无；

### 2、Example
Input: 1->1->2->3->3
Output: 1->2->3

<!-- more -->

### 3、Corner Case
1. 若head为NULL，直接返回head；
2. 若head为单结点，直接返回head；

## 二、题解

### 1、Solution-1：Two Pointers
0. tail指针在左，cur指针在右；比较两者的val是否相同；

1. 思路：对于若干个有着重复val的结点，我们仅使用第一个，其余的跳过，不考虑；

2. tail指向当前链表（有序、无重复）的尾结点，初始化为head；

3. cur从head->next遍历，到NULL结束，逐一考虑每个结点；

4. 若cur是某一批的第一个结点，将其接到tail的后面；否则，跳过，考虑cur->next；

5. 最后，将tail后面补一个NULL即可；

6. Code(https://leetcode.com/submissions/detail/392594939/)
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
        
        ListNode* tail = head;
        
        for (ListNode* cur = head->next; cur != NULL; cur = cur->next) {
            if (cur->val != tail->val) {
                tail->next = cur;
                tail = tail->next;
            }
        }
        tail->next = NULL;
        
        return head;
    }
};
```

7. Code(https://leetcode.com/submissions/detail/392716835/)
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
        
        ListNode* tail = head;
        
        for (ListNode* cur = head->next; cur != NULL; ) {
            if (cur->val != tail->val) {
                tail->next = cur;
                tail = tail->next;
                cur = cur->next;
            } else {
                ListNode* temp = cur;
                cur = cur->next;
                delete temp;
            }
        }
        tail->next = NULL;
        
        return head;
    }
};
```