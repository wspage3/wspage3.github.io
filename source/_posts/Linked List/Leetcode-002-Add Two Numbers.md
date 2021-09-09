---
title: Leetcode-002.Add Two Numbers
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List 
- Math
---

## 一、题意

### 0、题目链接
[002.Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

### 1、Description
【输入】
给定两个非空链表l1、l2；分别代表一个非负整数；
链表形式为：每个结点代表一位；头结点代表最低位（个位），尾结点代表最高位；
例如：[2 -> 4 -> 3 -> NULL]，代表整数：342；
【输出】
计算这两个整数的和，并且以上述形式返回；
【要求】
无；

### 2、Example
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

<!-- more -->

### 3、Corner Case
1. n1、n2均大于0；
2. num1、num2均为非负数；
3. num1、num2均无前置0；除了0本身外；

## 二、题解

### 0、思考
1. 模拟数学中的加法运算：个位对齐；每次两个数各拿一位进行相加，若有进位保存下来；

### 1、Solution-1：Math
1. extra记录是否有进位，初始化为0；

2. 当l1还有结点、或l2还有结点、或extra为1的时候：进行一轮加法运算；

3. Code(https://leetcode.com/submissions/detail/347044788/)
AC
时间复杂度：O(max(n1, n2))
空间复杂度：O(1)
```C
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
 
struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2) {
    
    struct ListNode dummy = {-1, NULL};
    struct ListNode *tail = &dummy;
 
    int extra = 0;
    while (l1 != NULL || l2 != NULL || extra) {
        // 1. calculate
        int curSum = (l1 == NULL ? 0 : l1->val) + (l2 == NULL ? 0 : l2->val) + extra;
 
        // 2. new a node
        struct ListNode *temp = (struct ListNode*)malloc(sizeof(struct ListNode));
        temp->val = curSum % 10;
        temp->next = NULL;
 
        // 3. append
        tail->next = temp;
        tail = tail->next;
 
        // 4. prepare next loop
        extra = curSum / 10;
        l1 = (l1 == NULL ? NULL : l1->next);
        l2 = (l2 == NULL ? NULL : l2->next);
    }
 
    return dummy.next;
}
```

4. Code(https://leetcode.com/submissions/detail/289719293/)
AC
时间复杂度：O(max(n1, n2))
空间复杂度：O(1)
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode dummy(0);
        ListNode* cur = &dummy;
        int extra = 0;
        while (l1 != NULL || l2 != NULL || extra != 0) {
            int sum = (l1 != NULL ? l1->val : 0) + (l2 != NULL ? l2->val : 0) + extra;
            ListNode* next = new ListNode(sum % 10);
            cur->next = next;
            cur = cur->next;
            extra = sum / 10;
            l1 = (l1 != NULL ? l1->next : l1);
            l2 = (l2 != NULL ? l2->next : l2);
        }
        return dummy.next;
    }
};
```