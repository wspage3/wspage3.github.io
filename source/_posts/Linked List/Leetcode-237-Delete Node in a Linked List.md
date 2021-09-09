---
title: Leetcode-237.Delete Node in a Linked List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List 
---

## 一、题意

### 0、题目链接
[237.Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)

### 1、Description
【输入】
给定一个单链表中的结点node（非尾结点）。
【输出】
在原链表中删除该结点node。
【要求】
题目未给定链表的头结点；

### 2、Example
Input: head = [4,5,1,9], node = 1
Output: [4,5,9]
Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.

<!-- more -->

### 3、Corner Case
1. 结点node不是尾结点，且保证是链表中的合法结点；
2. 本题中，链表至少有两个结点；
3. 所有结点的value不重复；
4. 函数返回类型为void；

## 二、题解

### 0、思考
1. 根据单链表的特性，若给定结点node，我们可以以O(1)的时间复杂度，删除node的后继结点；

2. 而本题中，给定结点node，要求删除它本身；

3. 且未给定链表的head结点，也就是说，我们不能从head遍历一次，找到node的前驱结点，通过修改其指向来删除node；

4. 转换思路：要想使得node消失，可以对node变化，使node与node的后继节点一模一样，再删除node的后继结点；

### 1、Solution-1
1. 结构体变量直接赋值；删除的结点最好释放内存；

2. Code(https://leetcode.com/submissions/detail/280512957/)
AC
时间复杂度：O(1)
空间复杂度：O(1)
```C++
class Solution {
public:
    void deleteNode(ListNode* node) {
        ListNode* temp = node->next;
        *node = *temp;
        delete temp;
    }
};
```
