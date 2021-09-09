---
title: Leetcode-373.Find K Pairs with Smallest Sums
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Linked List
- Heap
- Priority Queue
---

## 一、题意

### 0、题目链接
[373.Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

### 1、Description
【输入】
给定两个整数数组nums1、nums2，大小分别为n1、n2；并且都是升序排列好的；
给定一个整数k；
【输出】
以二维数组的形式，输出k个数字对(x, y)。其中x是nums1的元素，y是nums2的元素；
要求使得：这k个数字对，对应的val(x + y)最小；
【要求】
无；

### 2、Example
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]] 

<!-- more -->

### 3、Corner Case
1. 若n1 == 0 || n2 == 0，则不管k是多少，返回空数组；
2. 若n1、n2都不为0，但k <= 0，返回空数组；
3. 考虑n1、n2、k都为正数的情况；

## 二、题解

### 0、思考
1. 一共有n1 * n2个数字对，挑选其中k个；
2. 根据题目描述，若k > n1 * n2，则返回n1 * n2个即可；即：k = min(k, n1 * n2)；

### 1、Solution-1：Min Heap
1. 处理边界情况：若n1或n2为0；若k小于等于0；都直接返回空数组。

2. 处理k过大的情况：k = min(k, n1 * n2)；

3. 观察：对于nums1的任意一个数字x，要使得(x + y)最小：那么一定是先从nums2[0]开始选择y，到nums2[n2 - 1]为止。因为nums2是升序的。
把这种先后选择的次序，抽象为一个链表。选择完成一个结点后，考虑其后继结点。
那么本问题转化为：023.Merge k Sorted Lists。

4. 举个例子：nums1 = [1,7,11], nums2 = [2,4,6]
1：(1, 2) -> (1, 4) -> (1, 6)
7：(7, 2) -> (7, 4) -> (7, 6)
11：(11, 2) -> (11, 4) -> (11, 6)
可以看出，有n1个单链表，每个链表的长度为n2。将其merge，选择val最小的k个结点即可。

5. Code(https://leetcode.com/submissions/detail/330885880/)
AC
时间复杂度：O(M * log L)  // M = min(k, n1 * n2); L = min(k, n1);
空间复杂度：O(L)
```C++
class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        int n1 = nums1.size();
        int n2 = nums2.size();
        vector<vector<int>> ans;
        if (n1 == 0 || n2 == 0 || k <= 0) {
            return ans;
        }
        // i代表nums1的index
        // j代表nums2的index
        // {value, {i, j}}
        // pair<int, pair<int, int>>
        priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> minHeap;
        // 保持minHeap的大小为L：min(k, n1)。
        for (int i = 0; i <= min(k - 1, n1 - 1); i++) {
            minHeap.push({nums1[i] + nums2[0], {i, 0}});
        }
        // 需要遍历的次数为M：min(k, n1 * n2)。
        // 所以总体的时间复杂度为：O(M * log L)
        for (int num = 1; num <= min(k, n1 * n2); num++) {
            int curI = minHeap.top().second.first;
            int curJ = minHeap.top().second.second;
            minHeap.pop();
            ans.push_back({nums1[curI], nums2[curJ]});
            if (curJ == n2 - 1) {
                continue;
            }
            minHeap.push({nums1[curI] + nums2[curJ + 1], {curI, curJ + 1}});
        }
        return ans;
    }
};
```

