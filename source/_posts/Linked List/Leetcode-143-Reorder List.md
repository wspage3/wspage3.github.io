---
title: Leetcode-143.Reorder List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[143.Reorder List](https://leetcode.com/problems/reorder-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
L0→L1→…→Ln-1→Ln；
【输出】
将单链表按照：L0→Ln→L1→Ln-1→L2→Ln-2→...重新排列；
函数返回类型为void，新链表的头结点依然是head；
【要求】
无；

### 2、Example
Given 1->2->3->4, reorder it to 1->4->2->3.
Given 1->2->3->4->5, reorder it to 1->5->2->4->3.

<!-- more -->

### 3、Corner Case
1. 若结点数为0/1/2的时候，无需处理，直接返回；
2. 考虑结点数大于等于3的一般情况；

## 二、题解

### 0、思考
1. 在234题中，为了判断单链表是否为回文串：
首先，找出链表的中间结点；
其次，将链表的前半部分反转；
最后，依次比较两个子链表的每个结点；

### 1、Solution-1：Two Pointers + Reverse
1. 用双指针找出链表的中间结点：
如果中间结点有两个，slow最终指向左边那个，fast最终指向尾结点；
如果中间结点有一个，slow最终指向中间结点，fast最终指向NULL；

2. 记录right；

3. 将左边子链表截断；

4. 从right遍历，到NULL结束，反转这段链表；结束后，cur(或pre)指向反转后的链表头结点；

5. left(head)、right(cur/pre)逐一接入到tail后面；

6. Code(https://leetcode.com/submissions/detail/395461116/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void reorderList(ListNode* head) {
        if (head == NULL || head->next == NULL || head->next->next == NULL) {
            return;
        }
        
        // 1. find the mid, if two mid-node, choose the left one
        ListNode dummy(-1, head);
        ListNode* slow = &dummy;
        ListNode* fast = &dummy;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        ListNode* right = slow->next;
        slow->next = NULL;
        
        // 2. reverse the right half
        ListNode* pre = NULL;
        ListNode* cur = NULL;
        while (right != NULL) {
            cur = right;
            right = right->next;
            cur->next = pre;
            pre = cur;
        }
        
        // 3. merge
        // 左边长度 == 右边长度
        // 左边长度 == 右边长度 + 1
        dummy.next = NULL;
        ListNode* tail = &dummy;
        ListNode* left = head;
        right = cur;
        while (right != NULL) {
            tail->next = left;
            left = left->next;
            tail = tail->next;
            
            tail->next = right;
            right = right->next;
            tail = tail->next;
        }
        tail->next = left;
        
        return;
    }
};
```