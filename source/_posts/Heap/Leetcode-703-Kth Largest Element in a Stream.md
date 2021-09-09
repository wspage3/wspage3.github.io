---
title: Leetcode-703.Kth Largest Element in a Stream
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Design
- Heap
- Priority Queue
---

## 一、题意

### 0、题目链接
[703.Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

### 1、Description
【输入】
无；
【输出】
设计一个class KthLargest；
目的：找出一组数据流中，第k大的数字；
第k大：此处的第k大指的是将数据流中的数字从大到小排序后，第k个数字；
举个例子：[10, 10, 9, 8, 8]  第3大的数字为9，而不是8；
【要求】
构造函数：整数k、整数数组nums；初始化k以及数据流；
add函数：整数x；将整数x添加到数据流中，并且返回此时数据流中的第k大的数字；

### 2、Example
int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);   // returns 4
kthLargest.add(5);   // returns 5
kthLargest.add(10);  // returns 5
kthLargest.add(9);   // returns 8
kthLargest.add(4);   // returns 8

<!-- more -->

### 3、Corner Case
1. k >= 1
2. n >= k - 1
3. 当n == k - 1时：构造函数初始化后，第k大的元素还不能确定，需要后续的add()
4. 当n > k - 1时：构造初始化后，第k大的元素就知道了

## 二、题解

### 0、思考
1. 对于Kth的问题，同样考虑是否可以使用Heap来解决；

### 1、Solution-1：Min Heap
1. 维护一个大小为k的小顶堆；

2. 初始化：将nums中的n个数字放入大小为k的小顶堆中。若堆大小超过了k，则把堆顶元素去除。

3. add：将x放入小顶堆。若堆大小超过了k，则把堆顶元素去除。此时n一定大于等于k，第k大的数字就是堆顶的数字；

4. Code(https://leetcode.com/submissions/detail/330226399/)
AC
初始化时间复杂度：O(n * log k)  
add时间复杂度：O(log k)
空间复杂度：O(k)
```C++
class KthLargest {
private:
    int _k;
    priority_queue<int, vector<int>, greater<int>> minHeap;
public:
    KthLargest(int k, vector<int>& nums) {
        _k = k;
        for (int x : nums) {
            minHeap.push(x);
            if (minHeap.size() > _k) {
                minHeap.pop();
            }
        }
    }
    
    int add(int val) {
        minHeap.push(val);
        if (minHeap.size() > _k) {
            minHeap.pop();
        }
        return minHeap.top();
    }
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

