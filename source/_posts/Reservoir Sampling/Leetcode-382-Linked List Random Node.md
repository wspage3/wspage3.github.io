---
title: Leetcode-382.Linked List Random Node
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Reservoir Sampling
---

## 一、题意

### 0、题目链接
[382.Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
【输出】
在链表中随机选择一个结点，返回其value；
【要求】
使得链表中每一个结点被选择的概率都相等；
如果链表非常大，只允许对其遍历一次，且不使用额外空间。如何解决？

### 2、Example
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. 
// Each element should have equal probability of returning.
solution.getRandom();

<!-- more -->

### 3、Corner Case
1. head != NULL；
2. head->next == NULL时，返回head->val即可；

## 二、题解

### 0、思考
1. 对于一个有n个结点的链表list来说，每一个结点被选择的概率都应该是：1 / n。

### 1、Solution-1：Brute Force(Two-round)
1. 从head开始遍历链表list的每一个结点，记录其总的结点数目n。

2. 生成一个在范围[0, n - 1]的随机数x，代表链表中每个结点的index。区间内每个数字生成的概率都是相等的。

3. 再次从head开始遍历链表list，直到结点list[x]停止，返回其value即可。

4. 缺点：需要遍历两次链表。如果链表的规模非常大的时候，较耗时。

5. Code(https://leetcode.com/submissions/detail/321496335/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
private:
    ListNode* myHead;
    int len;
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        //通常来说，应该只播种一次随机数生成器，在程序开始处，任何到 rand() 的调用前。
        //不应重复播种，或每次冀愿生成新一批随机数时重播种。
        srand(time(0));
        myHead = head;
        len = 0;
        ListNode* cur = head;
        while (cur != NULL) {
            len++;
            cur = cur->next;
        }
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        int step = rand() % len;
        ListNode* cur = myHead;
        while (step--) {
            cur = cur->next;
        }
        return cur->val;
    }
};
```

### 2、Solution-2：Reservoir Sampling(One-round)
1. 变量num记录当前是链表list的第几个结点。

2. 变量ans记录"天选之子"的value字段，也就是要求的答案。

3. 从head开始遍历链表list，到第i个结点的时候，以1 / i的概率选择它；以(i - 1) / i的概率保持不变。

4. 举个例子：链表为[a->b->c->d]
i = 1：1 / 1的概率选择a->val。此时P(a) = 1 / 1；
i = 2：1 / 2的概率选择b->val。此时P(b) = 1 / 2，P(a) = (1) * (1 / 2) = 1 / 2；
i = 3：1 / 3的概率选择c->val。此时P(c) = 1 / 3，P(a) = P(b) = (1 / 2) * (2 / 3) = 1 / 3； 
i = 4：1 / 4的概率选择d->val。此时P(d) = 1 / 4，P(a) = P(b) = P(c) = (1 / 3) * (3 / 4) = 1 / 4；

5. 问题转化为：如何确定1 / n的概率呢？
rand() % n 会生成[0, n - 1]范围内的随机数，每个数出现的概率为1 / n。

6. Code(https://leetcode.com/submissions/detail/321531463/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
private:
    ListNode* myHead;
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        srand(time(0));
        myHead = head;
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        ListNode* cur = myHead->next;
        int num = 2;
        int ans = myHead->val;
        while (cur != NULL) {
            int r = rand() % num;
            if (r == 0) {
                ans = cur->val;
            }
            cur = cur->next;
            num++;
        }
        return ans;
    }
};
```

