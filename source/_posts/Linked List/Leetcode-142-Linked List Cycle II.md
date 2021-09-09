---
title: Leetcode-142.Linked List Cycle II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[142.Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
【输出】
判断该单链表中是否存在环路；
如果存在，找出入环点（环路开始的那个结点）；否则，返回NULL；
【要求】
O(1)的空间复杂度；

### 2、Example
Input: head = [3,2,0,-4], pos = 1
Output: true
注：此处的pos指链表的尾结点指向的结点；下标从0开始；
也就是说，尾结点(-4)指向2，形成环路；
返回2；

<!-- more -->

### 3、Corner Case
1. 若head为NULL，无环路；
2. 若head为单结点，且指向NULL，无环路；
3. 若一个结点指向自己本身，则形成自环，返回其本身；

## 二、题解

### 1、Solution-1：Two Pointers
1. hasCycle初始化为false；

2. slow、fast均从head出发，若相遇，更新hasCycle并break；

3. 设head到入环点的距离为a；
入环点到相遇点的距离为b；
相遇点到入环点的距离为c；

4. 若存在环路，则
slow走过的距离是：a + b
fast走过的距离是：a + b + k(b + c)，k >= 1；
则：2(a + b) = a + b + k(b + c)，k >= 1；
当k为1时：a = c；
当k为2时：a = c + (b + c)；
当k为3时：a = c + 2(b + c)；

5. slow从head出发，fast继续从相遇点出发；
两者每次均走1步，最后的相遇点就是入环点；

6. Code(https://leetcode.com/submissions/detail/390770270/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (head == NULL || head->next == NULL) {
            return NULL;
        }
        
        bool hasCycle = false;
        
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                hasCycle = true;
                break;
            }
        }
        
        if (hasCycle == false) {
            return NULL;
        }
        
        slow = head;
        while (slow != fast) {
            slow = slow->next;
            fast = fast->next;
        }
        
        return slow;
        
    }
};
```