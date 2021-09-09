---
title: Leetcode-203.Remove Linked List Elements
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[203.Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
给定一个整数val；
【输出】
将单链表中值为val的结点删除，返回新链表的头结点；
【要求】
无；

### 2、Example
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5

<!-- more -->

### 3、Corner Case
1. 若head为NULL，直接返回head；

## 二、题解

### 1、Solution-1：Two Pointers
0. tail指针在左，cur指针在右；比较val与cur->val是否相等；

1. 由于头结点可能被删除，所以使用虚拟头结点dummy；

2. tail指向新链表的尾结点，初始化指向dummy；

3. cur从head开始，到NULL结束，依次考虑cur是否满足条件：
若cur满足：将cur接在tail后面，更新tail；
若cur不满足：将cur删除；

4. 最后，将tail后面补一个NULL即可；

5. Code(https://leetcode.com/submissions/detail/392705491/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (head == NULL) {
            return head;
        }
        
        ListNode dummy(-1, head);
        ListNode* tail = &dummy;
        
        for (ListNode* cur = head; cur != NULL; cur = cur->next) {
            if (cur->val != val) {
                tail->next = cur;
                tail = tail->next;
            }
        }
        tail->next = NULL;
        
        return dummy.next;
    }
};
```

7. Code(https://leetcode.com/submissions/detail/392710440/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (head == NULL) {
            return head;
        }
        
        ListNode dummy(-1, head);
        ListNode* tail = &dummy;
        
        for (ListNode* cur = head; cur != NULL; ) {
            if (cur->val != val) {
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
        
        return dummy.next;
    }
};
```