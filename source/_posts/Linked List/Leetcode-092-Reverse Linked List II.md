---
title: Leetcode-092.Reverse Linked List II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List 
---

## 一、题意

### 0、题目链接
[092.Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

### 1、Description
【输入】
给定一个单链表；
给定一个区间范围[m, n]；
【输出】
将单链表中的第m个结点到第n个结点构成的这段子链表反转，返回反转后的链表头结点；
【要求】
无；

### 2、Example
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL

<!-- more -->

### 3、Corner Case
1. 链表为NULL时，无需操作。
2. 链表为单节点时，无需操作。
3. 若m == n，无需操作。
4. 本题中的编号，从1开始；
5. 要求one-pass，即只遍历一次链表；
6. m、n均合法：1 ≤ m ≤ n ≤ length of list；

## 二、题解

### 1、Solution-1：Iterative
1. tail指向Node(m - 1)；

2. start指向Node(m)；

3. 开始迭代方式的反转：head从Node(m)开始；cur指向当前考虑的结点；pre指向已经反转好的头结点；

4. 拼接：
tail向后指向cur(或pre)；
start向后指向head；

5. Code(https://leetcode.com/submissions/detail/389871507/)
AC
时间复杂度：O(n)
空间复杂度：O(1) 
```C++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (head == NULL || head->next == NULL || m == n) {
            return head;
        }
        
        // 1. dummy结点接在head前面；
        ListNode dummy(-1, head);
        ListNode* tail = &dummy;
        
        // 2. 循环m - 1次，tail后移；
        // 循环结束后，tail指向Node(m - 1)；
        for (int cnt = 1; cnt <= m - 1; cnt++) {
            tail = tail->next;
        }
 
        // 3. 开始reverse
        ListNode* start = tail->next;
        ListNode* pre = NULL;
        ListNode* cur = NULL;
        head = tail->next;
        for (int cnt = 1; cnt <= n - m + 1; cnt++) {
            cur = head;
            head = head->next;
            cur->next = pre;
            pre = cur;
        }
        
        // 4. 拼接
        tail->next = cur;
        start->next = head;        
        
        return dummy.next;
    }
};
```