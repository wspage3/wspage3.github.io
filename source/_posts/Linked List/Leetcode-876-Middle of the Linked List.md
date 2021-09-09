---
title: Leetcode-876.Middle of the Linked List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
---

## 一、题意

### 0、题目链接
[876.Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

### 1、Description
【输入】
给定一个非空的单链表，头结点为head；
【输出】
输出单链表的中间结点；
【要求】
若单链表的长度为奇数：中间结点有一个，输出即可；
若单链表的长度为偶数：中间结点有两个，输出靠右边那个；

### 2、Example
Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.

<!-- more -->

### 3、Corner Case
1. 链表非空；
2. 链表节点数目n：1 <= n <= 100；

## 二、题解

### 1、Solution-1：Brute Force(Two-round)
1. 首先遍历一次链表，统计出结点数目n；

2. 若n为奇数：如5，[1, 2, 3, 4, 5]。则head往后跳(5 / 2)步，返回head即可。
若n为偶数，如8，[1, 2, 3, 4, 5, 6, 7, 8]。则head往后跳(8 / 2)步，返回head即可。

3. Code(https://leetcode.com/submissions/detail/331607864/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        int n = 0;
        ListNode* cur = head;
        while (cur != NULL) {
            n++;
            cur = cur->next;
        }
        for (int num = 1; num <= n / 2; num++) {
            head = head->next;
        }
        return head;
    }
};
```

### 2、Solution-2：Two Pointers(One-round)
1. 双指针法，使得遍历一次链表就行。

2. slow、fast都初始化为head。slow每次走一步，fast每次走两步。

3. 一定是fast先碰到NULL，所以循环结束条件检测fast结点的状态即可。

4. 由于有赋值语句：fast = fast->next->next；需要保证fast非空；同时fast->next需要非空；

5. 举个例子：
n = 1：[1]。不进入循环，返回slow(1)即可。
n = 2：[1, 2]。循环一次，返回slow(2)即可。
n = 3：[1, 2, 3]。循环一次，返回slow(2)即可。
n = 4：[1, 2, 3, 4]。循环两次，返回slow(3)即可。
n = 5：[1, 2, 3, 4, 5]。循环两次，返回slow(3)即可。

6. Code(https://leetcode.com/submissions/detail/331608897/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```