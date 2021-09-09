---
title: Leetcode-234.Palindrome Linked List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Two Pointers
- Stack
---

## 一、题意

### 0、题目链接
[234.Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
【输出】
判断该单链表是否为一个回文串；
【要求】
O(n)的时间复杂度、O(1)的空间复杂度；

### 2、Example
Input: 1->2->2->1
Output: true

<!-- more -->

### 3、Corner Case
1. 若head为NULL、或者head为单结点时，认为是回文串；
2. 考虑结点数大于等于2的一般情况；

## 二、题解

### 1、Solution-1：Two Pointers + Stack
1. 用双指针找出链表的中间结点：
如果中间结点有两个，slow最终指向左边那个，fast最终指向尾结点；
如果中间结点有一个，slow最终指向中间结点，fast最终指向NULL；

2. 与此同时，将slow指向的结点依次压入栈s中；

3. 若此时fast为NULL，将栈顶元素去除；

4. fast从slow->next开始，依次与栈顶元素对比；出栈；fast后移；直到fast为NULL；

5. 返回答案；

6. Code(https://leetcode.com/submissions/detail/390378303/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return true;
        }
        
        stack<int> s;
        ListNode dummy(-1, head);
        ListNode* slow = &dummy;
        ListNode* fast = &dummy;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            s.push(slow->val);
            fast = fast->next->next;
        }
        
        if (fast == NULL) {
            s.pop();
        }
        
        fast = slow->next;
        while (fast != NULL) {
            if (s.top() != fast->val) {
                return false;
            }
            s.pop();
            fast = fast->next;
        }
        
        return true;
    }
};
```

### 2、Solution-2：Two Pointers + Reverse
1. 用双指针找出链表的中间结点：
如果中间结点有两个，slow最终指向左边那个，fast最终指向尾结点；
如果中间结点有一个，slow最终指向中间结点，fast最终指向NULL；

2. 记录rightStart；

3. 从head遍历，到slow结束，反转这段链表；结束后，cur(或pre)指向反转后的链表头结点；

4. 如果原链表长度是奇数的话（fast为NULL），cur后移一次；

5. cur、rightStart逐一比较；

6. Code(https://leetcode.com/submissions/detail/390409959/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return true;
        }
        
        // 1. find the mid(slow)
        ListNode dummy(-1, head);
        ListNode* slow = &dummy;
        ListNode* fast = &dummy;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        // right start point: rightStart
        ListNode* rightStart = slow->next;
        
        // 2. reverse[head...slow]
        ListNode* pre = NULL;
        ListNode* cur = NULL;
        while (head != rightStart) {
            cur = head;
            head = head->next;
            cur->next = pre;
            pre = cur;
        }
        
        // left start point: cur
        if (fast == NULL) {
            cur = cur->next;
        }
        
        // 3. compare
        while (rightStart != NULL) {
            if (rightStart->val != cur->val) {
                return false;
            }
            rightStart = rightStart->next;
            cur = cur->next;
        }
        
        return true;
    }
};
```