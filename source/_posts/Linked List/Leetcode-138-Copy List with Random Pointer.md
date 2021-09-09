---
title: Leetcode-138.Copy List with Random Pointer
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Hash Table
---

## 一、题意

### 0、题目链接
[138.Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

### 1、Description
【输入】
给定一个复杂单链表，头结点为head；
本题中的单链表有一个额外的指针random，指向链表中的某个结点，或者指向NULL；
【输出】
对上述复杂单链表进行“深拷贝”，返回新链表的头结点；
“深拷贝”：申请额外的内存空间存储链表；
【要求】
无；

### 2、Example
Input: head = [1, 2, 3, 4]
Output: [1', 2', 3', 4']

<!-- more -->

### 3、Corner Case
1. 若head为NULL时，返回NULL即可；
2. 考虑链表不为空的一般情况；

## 二、题解

### 0、思考
1. 对于普通单链表的“深拷贝”来说：每次遍历一个结点cur，然后new一个结点clone，并将clone接入在tail后面即可；

2. 对于“复杂”单链表来说，由于某个结点的random指针可能指向它后面若干个位置的结点，所以new完并不能处理好random指针域；

### 1、Solution-1：Hash Table
1. 思路：对于每一个cur和clone，使用哈希表来存储其关联关系；

2. 首先，cur从head开始遍历，到NULL结束：逐一申请结点clone，并将关联关系记录在哈希表m中；

3. 然后，cur从head开始遍历，到NULL结束：逐一处理m[cur]和next域和random域；

4. 最后，返回m[head]即可；

5. Code(https://leetcode.com/submissions/detail/396578792/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == NULL) {
            return NULL;
        }
        
        unordered_map<Node*, Node*> m;
        
        Node* cur = head;
        while (cur != NULL) {
            Node* clone = new Node(cur->val, NULL, NULL);
            m[cur] = clone;
            cur = cur->next;
        }
        
        cur = head;
        while (cur != NULL) {
            m[cur]->next = m[cur->next];
            m[cur]->random = m[cur->random];
            cur = cur->next;
        }
        
        return m[head];
    }
};
```

### 2、Solution-2
1. 首先，cur遍历原链表，从head开始遍历，到NULL结束：逐一申请结点clone，并将clone接到cur的后面；
例如，原链表为：[1, 2, 3]，经过此步骤，变为：[1, 1', 2, 2', 3, 3']；

2. 然后，cur遍历原链表，从head开始遍历，到NULL结束：逐一处理cur->next的random域；

3. 接着，将上述一条长链表拆分为两条长度相同的短链表；
使用虚拟结点dummy，tail指针初始化指向dummy，不断将克隆结点接入到tail的后面；

4. 最后，返回dummy.next即可；

5. Code(https://leetcode.com/submissions/detail/396583592/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == NULL) {
            return NULL;
        }
        
        Node* cur = head;
        while (cur != NULL) {
            Node* clone = new Node(cur->val);
            Node* next = cur->next;
            cur->next = clone;
            clone->next = next;
            cur = next;
        }
        
        cur = head;
        while (cur != NULL) {
            if (cur->random != NULL) {
                cur->next->random = cur->random->next;
            }
            cur = cur->next->next;
        }
        
        Node dummy(-1);
        Node* tail = &dummy;
        cur = head;
        while (cur != NULL) {
            Node* next = cur->next->next;
            tail->next = cur->next;
            tail = tail->next;
            cur->next = next;
            cur = next;
        }
        return dummy.next;
    }
};
```