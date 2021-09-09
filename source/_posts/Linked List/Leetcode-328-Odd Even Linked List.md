---
title: Leetcode-328.Odd Even Linked List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[328.Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
L1→…→Ln-1→Ln；
【输出】
将单链表按照：L1→L3→L5→...→L2→L4→L6→...重新排列，返回新链表的头结点；
【要求】
O(n)的时间复杂度，O(1)的空间复杂度；

### 2、Example
Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 10^4；
2. 若n为0/1/2的时候，无需处理，直接返回head；
3. 考虑n大于等于3的一般情况；

## 二、题解

### 1、Solution-1：Two Pointers
1. 记录第二段链表的头结点evenHead：head->next；

2. 指针odd指向第一段链表的尾结点（初始化head）；
指针even指向第二段链表的尾结点（初始化head->next）；

3. odd始终在even前面，需要对even进行判断：
当even为NULL：此时n为奇数，break；
当even的后继结点为NULL：此时n为偶数，break；

4. 边界条件：
n为奇数，如5：odd最终指向第一段链表的最后一个结点，even指向第二段链表的NULL结点；将evenHead拼接到odd后面即可；
n为偶数，如6：odd最终指向第一段链表的最后一个结点，even指向第二段链表的最后一个结点；将evenHead拼接到odd后面即可；

5. Code(https://leetcode.com/submissions/detail/395931645/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (head == NULL || head->next == NULL || head->next->next == NULL) {
            return head;
        }
        
        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* evenHead = head->next;
        while (even != NULL && even->next != NULL) {
            odd->next = odd->next->next;
            even->next = even->next->next;
            odd = odd->next;
            even = even->next;
        }
        odd->next = evenHead;
        
        return head;
    }
};
```