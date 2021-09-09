---
title: Leetcode-725.Split Linked List in Parts
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
---

## 一、题意

### 0、题目链接
[725.Split Linked List in Parts](https://leetcode.com/problems/split-linked-list-in-parts/)

### 1、Description
【输入】
给定一个单链表list，结点数为n，头结点为root；
给定一个正整数k；
【输出】
将list分成k个片段，每个片段都是一个小链表；
以一维数组的形式返回，数组每个元素是一个片段的头结点；
【要求】
1. 每一片段的长度尽可能的相等：任意两个片段的长度差不得大于1；
2. 前一片段的长度不得小于后一片段的长度；
3. 需要保持有序：k个片段的顺序需要与其在list中出现的顺序一致；


### 2、Example
Input: 
root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
Output: [[1, 2, 3, 4], [5, 6, 7], [8, 9, 10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 1000；
2. 1 <= k <= 50；

## 二、题解

### 0、思考
1. n个结点，分成k个片段：每个片段长度为n / k，还剩余n % k个结点，分配给前n % k个片段，每个片段1个即可；
这样可以保证：任意两个片段的长度差最多为1；而且前一片段的个数不小于后一片段的个数；
如：n = 20，k = 3；分成：[7, 7, 6]

### 1、Solution-1：Brute Force
1. 首先，统计list的结点数目n；

2. 每一片段长度暂定：n / k；

3. 将剩余的n % k个结点，分给前n % k个片段，每个片段分得1个；

4. Code(https://leetcode.com/submissions/detail/396509200/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<ListNode*> splitListToParts(ListNode* root, int k) {
        vector<ListNode*> ans(k, NULL);
        if (root == NULL) {
            return ans;
        }
        
        int n = 0;
        ListNode* temp = root;
        while (temp != NULL) {
            n++;
            temp = temp->next;
        }
        
        int len = n / k;
        int remainCnt = n % k;
        
        ListNode* cur = root;
        ListNode* pre = NULL;
        
        for (int i = 0; i <= k - 1; i++) {
            ans[i] = cur;
            for (int j = 1; j <= len + (remainCnt > 0); j++) {
                pre = cur;
                cur = cur->next;
            }
            pre->next = NULL;
            remainCnt--;
            
            if (cur == NULL) {
                break;
            }
        }
        
        return ans;
    }
};
```