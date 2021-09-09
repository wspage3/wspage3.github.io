---
title: Leetcode-147.Insertion Sort List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Sort
---

## 一、题意

### 0、题目链接
[147.Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
【输出】
对单链表进行插入排序，返回排序后的头结点；
【要求】
对原链表进行就地操作，不申请额外的空间存储链表；

### 2、Example
Input: 4->2->1->3
Output: 1->2->3->4

<!-- more -->

### 3、Corner Case
1. 若链表结点数为0/1时，无需处理，直接返回head；
2. 考虑链表结点数大于等于2的一般情况；

## 二、题解

### 1、Solution-1：Insertion Sort
1. 插入排序：
初始化：有序列表仅有一个元素(0)；
遍历：依次考察其余的元素(1, n - 1)，用O(n)的时间复杂度找出当前结点应该插入的位置；
整体时间复杂度为O(n * n)；

2. 由于当前结点可能插入到有序链表的头部，所以使用虚拟节点dummy，放在当前有序链表的头部；

3. 初始化：
dummy后接head；
cur指向head->next;
将有序链表截断；

4. 遍历：
cur指针用于遍历，考察每一个结点；
pos指针指向第一个满足插入条件的位置的前驱；
将cur插入到pos、pos->next中间；

5. Code(https://leetcode.com/submissions/detail/396891205/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        ListNode dummy(-1, head);
        ListNode* cur = head->next;
        head->next = NULL;
        
        while (cur != NULL) {
            ListNode* next = cur->next;
            ListNode* pos = &dummy;
            while (pos->next != NULL && pos->next->val < cur->val) {
                pos = pos->next;
            }
            cur->next = pos->next;
            pos->next = cur;
            cur = next;
        }
        
        return dummy.next;
    }
};
```
