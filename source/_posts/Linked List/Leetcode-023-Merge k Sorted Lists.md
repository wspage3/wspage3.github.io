---
title: Leetcode-023.Merge k Sorted Lists
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Sort
- Heap
- Priority Queue
- Divide and Conquer
---

## 一、题意

### 0、题目链接
[023.Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

### 1、Description
【输入】
给定k个单链表，单链表都是有序的（非降序）；
输入以数组的形式：数组的每个元素为一个单链表的头结点；
【输出】
将这k个单链表合并成一个单链表，同时保持有序的特点，返回头结点；
【要求】
分析每种方法的时间、空间复杂度；

### 2、Example
Input: 
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6


<!-- more -->

### 3、Corner Case
1. k：[0, 10 ^ 4]，单链表的个数
2. n：[0, 500]，单链表的长度
3. N：[0, 10 ^ 4]，所有结点的个数
4. 当k为0时，直接返回NULL；
5. 当k为1时，直接返回lists[0]；
6. 当lists[i]为NULL的时候，也就是该链表为空，将其跳过，考虑lists[i + 1]即可；

## 二、题解

### 1、Solution-1：Min Heap
1. 使用虚拟头结点dummy，方便返回结果，tail初始化指向dummy；

2. 每次从k个链表中挑选一个最小的，接在tail的后面；

3. 用一个大小为k的最小堆，可以在O(1)的时间找出最小结点；

4. 初始化最小堆：将k个头结点放入minHeap中，若有NULL，跳过；

5. 每次选一个最小的，抛出一个，进入一个；

6. 当minHeap为空时，所有结点都考虑过了；

7. Code(https://leetcode.com/submissions/detail/392204483/)
AC
时间复杂度：O(N * log k)
空间复杂度：O(k)
```C++
class Solution {
private:
    struct myGreater {
        bool operator()(const ListNode* a, const ListNode* b) {
            return a->val > b->val;
        }
    };
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int k = lists.size();
        if (k == 0) {
            return NULL;
        }
        if (k == 1) {
            return lists[0];
        }
        
        ListNode dummy(-1, NULL);
        ListNode* tail = &dummy;
        priority_queue<ListNode*, vector<ListNode*>, myGreater> minHeap;
        for (int i = 0; i <= k - 1; i++) {
            if (lists[i] == NULL) {
                continue;
            }
            minHeap.push(lists[i]);
        }
        while (!minHeap.empty()) {
            // 1. 拼接
            ListNode* temp = minHeap.top();
            tail->next = temp;
            tail = tail->next;
            // 2. 挤出堆顶
            minHeap.pop();
            // 3. 添加新的
            if (temp->next == NULL) {
                continue;
            }
            minHeap.push(temp->next);
        }
        
        return dummy.next;
    }
};
```

### 2、Solution-2：Sort
1. 将所有结点存放在一个数组中；

2. 自定义sort比较器，对数组排序；

3. 读取数组，拼接成链表；

4. Code(https://leetcode.com/submissions/detail/325942786/)
AC
时间复杂度：O(N * log N)   // 其中N为k个单链表的结点总数
空间复杂度：O(N)

### 3、Solution-3：Merge one by one
1. 对list[0]、list[1]进行合并；

2. 合并的结果再与list[2]进行合并；

3. 直到k个单链表合并成一个单链表；

4. Code(https://leetcode.com/submissions/detail/325950658/)
AC
时间复杂度：O(N * k)   // 具体分析见官方题解
空间复杂度：O(1)

### 4、Solution-4：Divide and Conquer
1. 首先将k个单链表两两一组进行merge，使得k = (k + 1) / 2；例如10变成5，13变成7。

2. 循环上述过程，使得k最终为1。结束。

3. 本质上是一个：自底向上的分治过程，而且是迭代版本的，非递归。

4. Code(https://leetcode.com/submissions/detail/392215389/)
AC
时间复杂度：O(N * log k)
空间复杂度：O(1)
```C++
class Solution {
private:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode dummy(0);
        ListNode* cur = &dummy;
        while (l1 != NULL && l2 != NULL) {
            if (l1->val <= l2->val) {
                cur->next = l1;
                l1 = l1->next;
            } else {
                cur->next = l2;
                l2 = l2->next;
            }
            cur = cur->next;
        }
        cur->next = (l1 == NULL ? l2 : l1);
        return dummy.next;
    }
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int k = lists.size();
        if (k == 0) {
            return NULL;
        }
        if (k == 1) {
            return lists[0];
        }
        
        int step = 1;
        
        while (step < k) {
            for (int i = 0; i <= k - 1 - step; i += 2 * step) {
                // i + step 需要保证在[0, n - 1]范围内，所以：i <= k - 1 - step
                lists[i] = mergeTwoLists(lists[i], lists[i + step]);
            }
            step = step * 2;
        }
        
        return lists[0];
    }
};
```