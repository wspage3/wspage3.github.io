---
title: Leetcode-025.Reverse Nodes in k-Group
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
---

## 一、题意

### 0、题目链接
[025.Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
给定一个正整数k：1 <= k <= n；
【输出】
对单链表中的结点k个一组进行反转，返回新链表的头结点；
当结点数不足k个时，不进行反转；
【要求】
O(1)的空间复杂度；

### 2、Example
Given this linked list: 1->2->3->4->5
For k = 2, you should return: 2->1->4->3->5
For k = 3, you should return: 3->2->1->4->5

<!-- more -->

### 3、Corner Case
1. 若链表结点数为0/1时，无需处理，直接返回head；
2. 若k == 1，无需处理，直接返回head；

## 二、题解

### 1、Solution-1：模拟
1. 首先，统计链表结点数n；

2. 其次，进行n / k轮循环；

3. 每一轮循环中：
记录反转起点start；
依次取k个结点，进行迭代方式的反转；
将反转后的链表接到tail后面；
更新tail；
更新k；

4. 将head接到tail后面：
head为NULL：n % k == 0；
head不为NULL：n % k != 0；

5. Code(https://leetcode.com/submissions/detail/398543094/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if (head == NULL || head->next == NULL || k == 1) {
            return head;
        }
        
        int n = 0;
        for (ListNode* cur = head; cur != NULL; cur = cur->next) {
            n++;
        }
        
        ListNode dummy(-1, NULL);
        ListNode* tail = &dummy;
        
        // 循环：n / k轮
        while (n >= k) {
            ListNode* start = head;
            ListNode* cur = NULL;
            ListNode* pre = NULL;
            for (int num = 1; num <= k; num++) {
                cur = head;
                head = head->next;
                cur->next = pre;
                pre = cur;
            }
            tail->next = cur;
            tail = start;
            n -= k;
        }
        tail->next = head;
        
        return dummy.next;
    }
};
```