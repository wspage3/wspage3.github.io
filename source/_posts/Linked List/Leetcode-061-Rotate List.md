---
title: Leetcode-061.Rotate List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[061.Rotate List](https://leetcode.com/problems/rotate-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
给定一个非负整数k；
【输出】
将单链表向右“旋转”k次，返回新链表的头结点；
“旋转”：将最后一个结点移动到链表的开头；
【要求】
无；

### 2、Example
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL

<!-- more -->

### 3、Corner Case
1. n == 0 || n == 1时，返回head即可；
2. k >= 0；
3. k == 0时，返回head即可；

## 二、题解

### 0、思考
1. 类似于189.Rotate Array：本题是对链表进行rotate；

2. 对k进行处理，防止k过大：k = k % n；

### 1、Solution-1：Two Pointers
1. 同189.Rotate Array相同，右旋转k次的本质都是：后k个结点移动到链表开头，前n - k个结点移动到链表结尾；

2. 首先，需要O(n)的时间来获取链表的结点数目n。此时，tail指向尾结点；

3. 其次，找出前n - k个结点的最后一个（preAns），后k个结点的第一个（ans）；

4. 最后，拼接：preAns后继置为NULL，tail后继置为head；

5. Code(https://leetcode.com/submissions/detail/396015124/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (head == NULL || head->next == NULL || k == 0) {
            return head;
        }
        
        // 1. 求出结点数n，以及尾结点
        int n = 1;
        ListNode* tail = head;
        while (tail->next != NULL) {
            n++;
            tail = tail->next;
        }
        
        // 2. 使得k在[0, n - 1]内
        k = k % n;
        if (k == 0) {
            return head;
        }
        
        // 3. 找到新的头结点ans，以及ans的前驱结点
        ListNode* preAns = head;
        for (int num = 1; num <= n - k - 1; num++) {
            preAns = preAns->next;
        }
        ListNode* ans = preAns->next;
        
        // 4. 拼接
        preAns->next = NULL;
        tail->next = head;
        
        return ans;
    }
};
```