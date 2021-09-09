---
title: Leetcode-160.Intersection of Two Linked Lists
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[160.Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

### 1、Description
【输入】
给定两个单链表，头结点分别为l1、l2；
【输出】
找出两个单链表的相交点，如果没有，返回NULL；
【要求】
函数不允许对两个单链表修改；
O(n)的时间复杂度，O(1)的空间复杂度；

### 2、Example
Input: listA = [1,9,1,2(addr = 0xffff0001),4], listB = [3,2(addr = 0xffff0001),4]
Output: Reference of the node with value = 2

<!-- more -->

### 3、Corner Case
1. l1、l2中均没有环路；
2. 若l1、l2中有一者为NULL，则无交点，返回NULL；
3. 考虑l1、l2均不为NULL的一般情况；

## 二、题解

### 1、Solution-1：Two Pointers
1. 如果存在相交点，则相交点以及后面的结点，是公共的，长度至少为1；
l1：[长度为a][intersection][长度为c]
l2：[长度为b][intersection][长度为c]

2. 指针p的遍历路径：l1l2：[a][intersection][c][b][intersection][c]
指针q的遍历路径：l2l1：[b][intersection][c][a][intersection][c]

3. 可以看出：
如果存在相交点，p、q同时跳若干次，最终相遇在相交点处，返回即可；
如果没有相交点，p、q同时跳a + b次，最终均到达NULL，返回即可；

4. Code(https://leetcode.com/submissions/detail/395507217/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* getIntersectionNode(ListNode* l1, ListNode* l2) {
        if (l1 == NULL || l2 == NULL) {
            return NULL;
        }
        
        ListNode* p = l1;
        ListNode* q = l2;
        while (p != q) {
            p = p->next;
            q = q->next;
            if (p == q) {
                // no intersection, a == b： 跳a步，p = q = NULL, return
                // intersection, a == b：跳a步, p = q != NULL, return
                // no intersection, a != b： 跳a + b步，p = q = NULL, return
                // intersection, a != b，a、b均不为0：跳a + b + c步, p = q != NULL, return
                return p; 
            }
            if (p == NULL) {
                p = l2;
            }
            if (q == NULL) {
                q = l1;
            }
        }
        
        return p; // intersection, a != b，a、b至少有一个为0，执行到此处；
    }
};
```