---
title: Leetcode-086.Partition List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Sort
---

## 一、题意

### 0、题目链接
[086.Partition List](https://leetcode.com/problems/partition-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
给定一个整数x；
【输出】
对单链表进行重排列，使得：链表左边元素均小于x，右边元素均大于等于x；
同时，左右两边内部元素的相对顺序，需要保持；
【要求】
无；

### 2、Example
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5

<!-- more -->

### 3、Corner Case
1. 若链表结点数为0/1时，无需处理，直接返回head；
2. 考虑链表结点数大于等于2的一般情况；

## 二、题解

### 1、Solution-1：Quick Sort
1. 快速排序中的partition过程；

2. Code(https://leetcode.com/submissions/detail/397389747/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        ListNode lDummy(-1, NULL);
        ListNode rDummy(-1, NULL);
        ListNode* lTail = &lDummy;
        ListNode* rTail = &rDummy;
        
        for (ListNode* cur = head; cur != NULL; cur = cur->next) {
            if (cur->val < x) {
                lTail->next = cur;
                lTail = lTail->next;
            } else {
                rTail->next = cur;
                rTail = rTail->next;
            }
        }
        
        lTail->next = NULL;
        rTail->next = NULL;
        
        lTail->next = rDummy.next;
        
        return lDummy.next;
    }
};
```