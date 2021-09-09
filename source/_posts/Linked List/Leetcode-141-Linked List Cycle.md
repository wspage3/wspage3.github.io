---
title: Leetcode-141.Linked List Cycle
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[141.Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
【输出】
判断该单链表中是否存在环路；
【要求】
O(1)的空间复杂度；

### 2、Example
Input: head = [3,2,0,-4], pos = 1
Output: true
注：此处的pos指链表的尾结点指向的结点；下标从0开始；
也就是说，尾结点(-4)指向2，形成环路；

<!-- more -->

### 3、Corner Case
1. 若head为NULL，无环路；
2. 若head为单结点，且指向NULL，无环路；
3. 若一个结点指向自己本身，则形成自环；

## 二、题解

### 1、Solution-1：Two Pointers
1. 要想形成环路，至少存在一个结点，它指向自己本身；

2. 如果链表中存在环路，则从head出发遍历，永不结束；

3. slow、fast均从head出发，slow每次走一步，fast每次走两步；
若链表中没有环路，一定是fast先达到NULL；
若链表中有环路，slow、fast一定会相遇；

4. Code(https://leetcode.com/submissions/detail/390417643/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (head == NULL || head->next == NULL) {
            return false;
        }
        
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                return true;
            }
        }
        
        return false;
    }
};
```