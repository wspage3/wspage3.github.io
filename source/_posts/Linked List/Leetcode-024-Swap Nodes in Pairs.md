---
title: Leetcode-024.Swap Nodes in Pairs
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[024.Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
L1→…→Ln-1→Ln；
【输出】
将单链表按照：L2→L1→L4→L3→...重新排列，返回新链表的头结点；
【要求】
无；

### 2、Example
Given 1->2->3->4, you should return the list as 2->1->4->3.
Given 1->2->3->4->5, you should return the list as 2->1->4->3->5.

<!-- more -->

### 3、Corner Case
1. 若n为0/1的时候，无需处理，直接返回head；
2. 考虑n大于等于2的一般情况；

## 二、题解

### 1、Solution-1：Two Pointers(Iterative)
1. 设置虚拟头结点dummy，tail指针指向结果链表的尾结点；

2. head指针用于遍历，当链表剩余结点不足两个时，跳出循环；

3. odd指针指向左边结点，even指针指向右边结点；

4. 拼接；

5. 更新head、tail：用于下一轮循环；

6. Code(https://leetcode.com/submissions/detail/395979322/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        ListNode dummy(-1, NULL);
        ListNode* tail = &dummy;
        
        ListNode* odd = NULL;
        ListNode* even = NULL;
        
        // 当不足两个结点时，跳出循环
        while (head != NULL && head->next != NULL) {
            // 左边结点记为odd，右边结点记为even
            odd = head;
            even = head->next;
            // 拼接
            tail->next = even;
            odd->next = even->next;
            even->next = odd;
            // 更新head、tail
            head = odd->next;
            tail = odd;
        }
        
        return dummy.next;
    }
};
```

### 2、Solution-2：Recursive
1. 递归结束条件：子链表结点个数不足2；

2. 左边结点为head，右边结点记为n；

3. 递归调用子链表：swapPairs(n->next)；

4. 拼接；返回n即可；

5. Code(https://leetcode.com/submissions/detail/395980681/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        ListNode* n = head->next;
        
        head->next = swapPairs(n->next);
        n->next = head;
        
        return n;
    }
};
```