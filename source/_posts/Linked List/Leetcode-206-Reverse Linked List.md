---
title: Leetcode-206.Reverse Linked List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List 
---

## 一、题意

### 0、题目链接
[206.Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

### 1、Description
【输入】
给定一个单链表；
【输出】
将其反转，返回反转后的链表头结点；
【要求】
无；

### 2、Example
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL

<!-- more -->

### 3、Corner Case
1. 链表为NULL时，无需操作；
2. 链表为单节点时，无需操作；
3. 递归法 + 迭代法；

## 二、题解

### 1、Solution-1：Recursive
1. 递归结束条件：当head为NULL或者单结点时，直接返回head；

2. 对于结点head：
首先，递归调用reverseList(head->next)，记录结果为newHead;
然后，将head放在其已经逆序好的链表末尾；
最后，返回newHead；

3. Code(https://leetcode.com/submissions/detail/279789541/)
AC
时间复杂度：O(n)
空间复杂度：O(n) // 递归开销
```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return newHead;
    }
};
```

### 2、Solution-2：Iterative
1. head：遍历链表，直到所有非空结点都被考察过；

2. cur：当前考察的结点；

3. pre：已经逆序好的头结点；

4. Code(https://leetcode.com/submissions/detail/389780451/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        ListNode* pre = NULL;
        ListNode* cur = NULL;
        while (head != NULL) {
            cur = head;
            head = head->next;
            cur->next = pre;
            pre = cur;
        }
        // 每轮loop结束后，cur、pre指向同一个node；
        return cur;
    }
};
```