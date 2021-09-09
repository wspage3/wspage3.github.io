---
title: 《剑指Offer》-022-链表中倒数第k个节点
categories: 
- [《剑指Offer》]
- [数据结构与算法]
tags: 
- Two Pointers
- Linked List
---

## 一、题意

### 0、题目链接
[《剑指Offer》-022-链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

### 1、Description
【输入】
一个单链表的头节点head；
一个整数k；
【输出】
求出该单链表的倒数第k个节点；
本题中，下标从1开始，即：头节点为第1个，尾节点为第n个；
【要求】
无；

### 2、Example
输入：
给定一个链表: 1->2->3->4->5, 和 k = 2；
输出：
返回链表 4->5；

<!-- more -->

### 3、Corner Case
1. k < 1 || k > n时，返回NULL；
2. 1 <= k <= n；

## 二、题解

### 1、Solution-1：Two-Rounds
1. 观察发现：
当下标从1开始时，倒数第k个节点就是：正数第n + 1 - k个节点；
当下标从0开始时，倒数第k个节点就是：正数第n - 1 - k个节点；

2. 首先，求出链表长度n；O(n)；

3. 其次，校验k的合法性；

4. 最后，求出第n + 1 - k个节点，即：跳n - k次；O(n)；
此时1 <= k <= n，跳的次数在[0, n - 1]内，不会出现指针异常；

5. Code(https://leetcode-cn.com/submissions/detail/82842914/)
AC
时间复杂度：O(n) //两轮
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        // 倒数第k = 正数第n + 1 - k
        ListNode* cur = head;
        int n = 0;
        while (cur != NULL) {
            n++;
            cur = cur->next;
        }
 
        if (k < 1 || k > n) {
            return NULL;
        }
 
        cur = head;
        for (int i = 1; i <= n - k; i++) {
            cur = cur->next;
        }
 
        return cur;
    }
};
```

### 2、Solution-2：Two Pointers
1. 使用虚拟头结点dummy；

2. 初始化：slow和fast都指向dummy；

3. fast先跳k次；

4. slow、fast同时跳；当fast刚好在NULL的时候，slow刚好在倒数第k个节点的位置上；

5. 边界情况考虑：
k < 1：函数开始即可判断；
k > n：fast跳k次，若跳完发现为NULL了，说明k > n了；

6. Code(https://leetcode-cn.com/submissions/detail/105974492/)
AC
时间复杂度：O(n) //一轮
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        if (head == NULL || k <= 0) {
            return NULL;
        }
 
        ListNode dummy(-1, head);
        ListNode* slow = &dummy;
        ListNode* fast = &dummy;
 
        for (int num = 1; num <= k; num++) {
            fast = fast->next;
            if (fast == NULL) {
                return NULL;
            }
        }
 
        while (fast != NULL) {
            fast = fast->next;
            slow = slow->next;
        }
        
        return slow;
    }
};

```