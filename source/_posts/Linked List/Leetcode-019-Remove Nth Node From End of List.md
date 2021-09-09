---
title: Leetcode-019.Remove Nth Node From End of List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[019.Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head。
给定一个整数k；
【输出】
删除单链表中的倒数第k个结点，返回此时单链表的头结点；
本题中，编号从1开始；
【要求】
one pass：仅遍历链表一次；

### 2、Example
Given linked list: 1->2->3->4->5, and k = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.

<!-- more -->

### 3、Corner Case
1. 题目保证k一定是合法的，即：1 <= k <= n；
2. head为NULL时，无需考虑；

## 二、题解

### 1、Solution-1：Brute Force(Two-round)
1. 求出链表的长度n；

2. 设置虚拟头结点dummy，tail指向它；

3. tail向后移动n - k次，此时tail指向target结点的前驱结点；

4. 通过修改前驱结点的指向，删除target结点；

5. 返回dummy的后继结点即可；

6. Code(https://leetcode.com/submissions/detail/390286016/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int k) {
        if (head == NULL) {
            return head;
        }
        // 1. count length of linked list
        int n = 0;
        ListNode* temp = head;
        while (temp != NULL) {
            n++;
            temp = temp->next;
        }
        
        // 2. find the node
        ListNode dummy(-1, head);
        ListNode* tail = &dummy;
        for (int num = 1; num <= n - k; num++) {
            tail = tail->next;
        }
        
        // 3. remove tail->next
        ListNode* target = tail->next;
        tail->next = target->next;
        delete target;
        
        // 4. return answer
        return dummy.next;
    }
};
```

### 2、Solution-2：Two Pointers(One-round)
1. 同Solution-1一样，我们只要找到target的前驱结点，问题就迎刃而解了；

2. 设置虚拟头结点dummy，slow、fast指向它；

3. fast先向后移动k + 1次；

4. slow、fast同时向后移动一次，直到fast为NULL；
此时slow指向的就是target的前驱结点；
将其删除即可；

5. 举个例子：
n = 5：dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL；
当k = 1时：fast移动2次，指向2；slow、fast同时移动4次，slow指向4，也就是5的前驱结点；删除5；
当k = 3时：fast移动4次，指向4；slow、fast同时移动2次，slow指向2，也就是3的前驱结点；删除3；
当k = 5时，fast移动6次，指向NULL；slow、fast同时移动0次；slow指向dummy，与就是1的前驱结点；删除1；

6. Code(https://leetcode.com/submissions/detail/390364596/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int k) {
        if (head == NULL) {
            return head;
        }
        
        ListNode dummy(-1, head);
        ListNode* slow = &dummy;
        ListNode* fast = &dummy;
        
        for (int num = 1; num <= k + 1; num++) {
            fast = fast->next;
        }
        
        while (fast != NULL) {
            slow = slow->next;
            fast = fast->next;
        }
        
        ListNode* target = slow->next;
        slow->next = target->next;
        delete target;
        
        return dummy.next;
    }
};
```