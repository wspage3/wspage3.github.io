---
title: Leetcode-1046.Last Stone Weight
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Heap
- Priority Queue
- Sort
---

## 一、题意

### 0、题目链接
[1046.Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)

### 1、Description
【输入】
给定一个长度为n的整数数组stones；代表有n块石头，第i块的重量为stones[i]。
【背景】
拿两块石头，重量分别为x和y，“碰一碰”，会出现以下两种情况：
若x == y，两块石头都会消失；
若x != y，则重量小的那块（如x）会消失，重量大的那块（如y）的重量会变成：y - x；
【输出】
每一次，拿两块最重的石头“碰一碰”；直到最终剩下一块石头，或没有石头；
求出最后一块石头的重量。若没有石头了，输出0；
【要求】
无；

### 2、Example
Input: [2,7,4,1,8,1]
Output: 1
Explanation: 
We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of last stone.

<!-- more -->

### 3、Corner Case
1. 1 <= stones.length <= 30
2. 1 <= stones[i] <= 1000
3. n == 1时：返回stones[0]即可。
4. n == 2时，返回abs(stones[0] - stones[1])即可。

## 二、题解

### 0、思考
1. 由于每次需要取最大的两个数，且stones中的数字是动态变化的。考虑用大顶堆来模拟。

### 1、Solution-1：Max Heap
1. 将stones中的所有数字放入大小为n的大顶堆中。

2. 每次取出堆顶（最大）两个数字，判断其是否相等。若不相等，再将差值加入到大顶堆中。

3. 当堆中元素个数小于等于1时，结束。

4. Code(https://leetcode.com/submissions/detail/332175574/)
AC
时间复杂度：O(n * log n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        int n = stones.size();
        if (n == 1) {
            return stones[0];
        }
        if (n == 2) {
            return abs(stones[0] - stones[1]);
        }
        int ans;
        priority_queue<int> maxHeap;
        for (int x : stones) {
            maxHeap.push(x);
        }
        while (maxHeap.size() >= 2) {
            int x = maxHeap.top();
            maxHeap.pop();
            int y = maxHeap.top();
            maxHeap.pop();
            if (x > y) {
                maxHeap.push(x - y);
            }
        }
        return maxHeap.size() == 1 ? maxHeap.top() : 0;
    }
};
```

### 2、Solution-2：Bucket Sort
1. 由于每块石头的重量：1 <= stones[i] <= 1000。

2. 创建1001个桶buckets，bucket[i]记录重量为i的石头有几个。

3. r从1000开始遍历，到1结束。找出一个最大重量的石头。然后l从r - 1开始遍历，到1结束，找出一个第二大重量的石头。

4. 时间复杂度分析：
外层循环（r）遍历N次，其中N = maximumWeight；
内层循环（l）最多遍历N次，考虑[1,1,1,1,1,...,1000]的情况，每次l都需要从r - 1遍历到1。
综上所述，时间复杂度为O(N * N)。
当N规模较小，而n较大时：可以同Solution-1进行比较，选择较优的方法；

5. Code(https://leetcode.com/submissions/detail/335073087/)
AC
时间复杂度：O(N * N)  // N = maximumWeight
空间复杂度：O(N)
```C++
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        int n = stones.size();
        if (n == 1) {
            return stones[0];
        }
        if (n == 2) {
            return abs(stones[0] - stones[1]);
        }
        
        int buckets[1001] = {0};
        for (int w : stones) {
            buckets[w]++;
        }
        int r = 1000;
        while (r >= 1) {
            if (buckets[r] % 2 == 0) {
                r--;
            } else {
                int l = r - 1;
                while (l >= 1 && buckets[l] == 0) {
                    l--;
                }
                if (l == 0) {
                    return r;
                }
                buckets[r] = 0;
                buckets[l]--;
                buckets[r - l]++;
                r--;
            }
        }
        return 0;
    }
};
```