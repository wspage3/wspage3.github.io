---
title: Leetcode-148.Sort List
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Sort
---

## 一、题意

### 0、题目链接
[148.Sort List](https://leetcode.com/problems/sort-list/)

### 1、Description
【输入】
给定一个单链表，头结点为head；
【输出】
对单链表进行排序，返回排序后的头结点；
【要求】
O(n * log n)的时间复杂度，O(1)的空间复杂度；

### 2、Example
Input: 4->2->1->3
Output: 1->2->3->4

<!-- more -->

### 3、Corner Case
1. 若链表结点数为0/1时，无需处理，直接返回head；
2. 考虑链表结点数大于等于2的一般情况；

## 二、题解

### 1、Solution-1：Insertion Sort
1. Code(https://leetcode.com/submissions/detail/396961898/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
private:
    ListNode* insertionSort(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        ListNode dummy(-1, head);
        ListNode* cur = head->next;
        head->next = NULL;
        
        while (cur != NULL) {
            ListNode* next = cur->next;
            ListNode* p = &dummy;
            while (p->next != NULL && p->next->val < cur->val) {
                p = p->next;
            }
            cur->next = p->next;
            p->next = cur;
            cur = next;
        }
        
        return dummy.next;
    }
public:
    ListNode* sortList(ListNode* head) {
        return insertionSort(head);
    }
};
```

### 2、Solution-2：Quick Sort
1. 快速排序：自顶向下的递归求解

2. 每一轮：
将链表分为左、中、右三个部分；
递归求左、右两部分；
将三个部分merge到一起；

3. Code(https://leetcode.com/submissions/detail/397301442/)
AC
时间复杂度：O(n * log n)
空间复杂度：O(log n)
```C++
class Solution {
private:
    // 给定一个单链表的头结点head，求尾结点tail
    ListNode* getTail(ListNode* head) {
        if (head == NULL) {
            return NULL;
        }
        
        ListNode* tail = head;
        while (tail->next != NULL) {
            tail = tail->next;
        }
        return tail;
    }
public:
    ListNode* sortList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        // 1. 将链表头结点作为快速排序时partition的target
        int target = head->val;
        
        // 2. 将链表分成三部分：左、中、右
        ListNode lDummy(-1, NULL);
        ListNode mDummy(-1, head);
        ListNode rDummy(-1, NULL);
        ListNode* lTail = &lDummy;
        ListNode* mTail = head;
        ListNode* rTail = &rDummy;
        
        // 3. 依次考察链表的其余结点
        ListNode* cur = head->next;
        while (cur != NULL) {
            if (cur->val < target) {
                lTail->next = cur;
                lTail = lTail->next;
            } else if (cur->val == target) {
                mTail->next = cur;
                mTail = mTail->next;
            } else {
                rTail->next = cur;
                rTail = rTail->next;
            }
            cur = cur->next;
        }
        
        // 4. 截断，使得成为三条单链表：
        // lDummy -> ... -> NULL
        // mDummy -> head -> ... -> NULL
        // rDummy -> ... -> NULL
        lTail->next = NULL;
        mTail->next = NULL;
        rTail->next = NULL;
        
        // 5. 对左右两条链表，进行递归求解（自顶向下的递归）
        lDummy.next = sortList(lDummy.next);
        rDummy.next = sortList(rDummy.next);
        
        // 6. 将三条单链表merge成一条单链表
        lTail = getTail(&lDummy);
        lTail->next = mDummy.next;
        mTail->next = rDummy.next;
        
        return lDummy.next;
    }
};
```

### 3、Solution-3：Merge Sort(Top-To-Down)
1. 归并排序：自顶向下的递归求解

2. 每一轮：
将链表分为左、右两个部分；
递归求左、右两部分；
将两个部分merge到一起；

3. Code(https://leetcode.com/submissions/detail/397319479/)
AC
时间复杂度：O(n * log n)
空间复杂度：O(log n)
```C++
class Solution {
private:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        // Leetcode-021-Merge Two Sorted Lists
    }
public:
    ListNode* sortList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        // 1. 将单链表分成左、右两部分
        ListNode dummy(-1, head);
        ListNode* slow = &dummy;
        ListNode* fast = &dummy;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        fast = slow->next;
        slow->next = NULL;
        
        // 2. 对左、右两部分求解
        slow = sortList(head);
        fast = sortList(fast);
        
        // 3. 将左、右两个有序链表merge起来
        return mergeTwoLists(slow, fast);
    }
};
```

### 3、Solution-3：Merge Sort(Bottom To Up)
1. 归并排序：自下而上的迭代求解

2. 首先，统计链表结点的数目n；

3. 其次，枚举有序区间的长度len，将长度为len的两个区间合并为一个长度为2 * len的有序区间；

4. Code(https://leetcode.com/submissions/detail/397371134/)
AC
时间复杂度：O(n * log n)
空间复杂度：O(1)
```C++
class Solution {
private:
    // 合并两个有序链表，返回合并后的头结点
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL || l2 == NULL) {
            return l1 == NULL ? l2 : l1;
        }
        
        ListNode dummy(-1, NULL);
        ListNode* tail = &dummy;
        while (l1 != NULL && l2 != NULL) {
            if (l1->val <= l2->val) {
                tail->next = l1;
                tail = tail->next;
                l1 = l1->next;
            } else {
                tail->next = l2;
                tail = tail->next;
                l2 = l2->next;
            }
        }
        tail->next = (l1 == NULL ? l2 : l1);
        
        return dummy.next;
    }
    
    // start指针往后移动k次：返回移动后的头结点
    // [1, 2, 3, 4, 5, 6], k = 4：[1,2,3,4][5,6]
    // [1, 2, 3, 4, 5], k = 4：   [1,2,3,4][5]
    // [1, 2, 3, 4], k = 4：      [1,2,3,4][]
    // [1, 2, 3], k = 4：         [1,2,3][]
    // [1, 2], k = 4：            [1,2][]
    // [1], k = 4：               [1][]
    // [], k = 4：                [][]
    ListNode* jump(ListNode* start, int k) {
        if (start == NULL) {
            return NULL;
        }
        
        for (int num = 1; num <= k - 1; num++) {
            start = start->next;
            if (start == NULL) {
                return NULL;
            }
        }
        ListNode* ans = start->next;
        start->next = NULL;
        return ans;
    }
    
    // 给定一个单链表的头结点head，求尾结点tail
    ListNode* getTail(ListNode* head) {
        if (head == NULL) {
            return NULL;
        }
        
        ListNode* tail = head;
        while (tail->next != NULL) {
            tail = tail->next;
        }
        return tail;
    }
public:
    ListNode* sortList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        
        int n = 0;
        for (ListNode* cur = head; cur != NULL; cur = cur->next) {
            n++;
        }
        
        ListNode dummy(-1, head);
        
        for (int len = 1; len < n; len *= 2) {
            // 每一轮循环，将长度为len的有序区间，两两合并
            ListNode* tail = &dummy;
            ListNode* cur = dummy.next;        // cur指针：从原链表第一个结点开始遍历
            while (cur != NULL) {
                ListNode* left = cur;              // left指针：指向第一个有序区间的头结点
                ListNode* right = jump(left, len); // right指针：指向第二个有序区间的头结点
                cur = jump(right, len);            // 更新cur，作为下轮遍历的起点
                tail->next = mergeTwoLists(left, right); // 将merge好的链表，接在tail后面
                tail = getTail(tail); // 更新tail
            }
            
        }
        
        return dummy.next;        
    }
};
```