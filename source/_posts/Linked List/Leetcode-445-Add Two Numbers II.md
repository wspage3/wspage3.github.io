---
title: Leetcode-445.Add Two Numbers II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List 
- Math
- Stack
---

## 一、题意

### 0、题目链接
[445.Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/)

### 1、Description
【输入】
给定两个非空链表l1、l2；分别代表一个非负整数；
链表形式为：每个结点代表一位；头结点代表最高位，尾结点代表最低位（个位）；
例如：[2 -> 4 -> 3 -> NULL]，代表整数：243；
【输出】
计算这两个整数的和，并且以上述形式返回；
【要求】
不可以修改原链表，即不允许反转链表；

### 2、Example
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
Explanation: 7243 + 564 = 7807.

<!-- more -->

### 3、Corner Case
1. n1、n2均大于0；
2. num1、num2均为非负数；
3. num1、num2均无前置0；除了0本身外；

## 二、题解

### 0、思考
1. Leetcode-002-Add Two Numbers中：
链表特定：头结点是数字的个位，尾结点是数字的最低位；
解决：模拟数学加法运算：每次拿个位数字（各自链表的头结点）计算，生成一个结点后插入要已有链表的尾部；

2. 可以对l1、l2进行反转，然后问题转换为LT002；

2. 本题中，解决思路是类似的：
只需要将链表中的元素从前到后压入栈中；
然后取栈顶元素即为个位数字；
生成一个结点后插入到已有链表的头部；

### 1、Solution-1：Stack + Math
1. Code(https://leetcode.com/submissions/detail/430874030/)
AC
时间复杂度：O(n1 + n2)
空间复杂度：O(n1 + n2)
```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> s1;
        stack<int> s2;
        ListNode* cur = l1;
        while (cur) {
            s1.push(cur->val);
            cur = cur->next;
        }
        cur = l2;
        while (cur) {
            s2.push(cur->val);
            cur = cur->next;
        }
        
        ListNode* head = NULL;
        
        int extra = 0;
        while (!s1.empty() || !s2.empty() || extra) {
            int num = (!s1.empty() ? s1.top() : 0) + (!s2.empty() ? s2.top() : 0) + extra;
            if (!s1.empty()) {
                s1.pop();
            }
            if (!s2.empty()) {
                s2.pop();
            }
            ListNode* cur = new ListNode(num % 10);
            extra = num / 10;
            cur->next = head;
            head = cur;
        }
        
        return head;
    }
};
```