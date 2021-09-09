---
title: Leetcode-347.Top K Frequent Elements
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
[347.Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

### 1、Description
【输入】
给定一个非空数组nums，大小为n；
给定一个正整数k；
【输出】
将nums中出现最频繁的前k个数字，以数组的形式返回；
【要求】
对返回的k个数字，无特定的顺序要求；
时间复杂度需要优于O(n * log n)；

### 2、Example
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

<!-- more -->

### 3、Corner Case
1. n >= 1
2. k合法：1 <= k <= number of unique elements
3. 题目数据保证：对于一个特定的k，答案是唯一的。
例如，k = 1时，若答案为x，x出现的次数为cntX。题目数据保证nums中不会有其它元素出现的次数也为cntX，从而造成答案的不唯一性。

## 二、题解

### 0、思考
1. 对于Top K的问题，考虑是否可以使用Heap来解决；

### 1、Solution-1：Min Heap
1. 统计每个数字出现的次数，使用一个哈希表存起来即可，时间复杂度O(n)；假设大小为m；

2. 维护一个大小为k的小顶堆；

3. 将哈希表中的m个数据插入小顶堆中。若堆大小超过了k，则把堆顶元素去除。循环结束后，留下的就是Top K的数字了。

4. Code(https://leetcode.com/submissions/detail/324424232/)
AC
时间复杂度：O(n * log k)  
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        int n = nums.size();
        if (n == 0) {
            return {};
        }
        
        unordered_map<int, int> cnt; // num to frequency
        for (int x : nums) {
            cnt[x]++;
        }
      
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        for (auto kv : cnt) {
            pq.push({kv.second, kv.first});
            if (pq.size() > k) {
                pq.pop();
            }
        }
        
        vector<int> ans;
        while (!pq.empty()) {
            ans.push_back(pq.top().second);
            pq.pop();
        }
        return ans;
    }
};
```

### 2、Solution-2：Max Heap
1. 统计每个数字出现的次数，使用一个哈希表存起来即可，时间复杂度O(n)；假设大小为m；

2. 维护一个大小为m - k的大顶堆；

3. 将哈希表中的m个数据插入大顶堆中。若堆大小超过了m - k，则把堆顶元素存到ans中。循环结束后，ans中的元素就是Top K的数字了。

4. Code(https://leetcode.com/submissions/detail/324426968/)
AC
时间复杂度：O(n * log(m - k))
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        int n = nums.size();
        if (n == 0) {
            return {};
        }
        
        unordered_map<int, int> cnt; // num to frequency
        for (int x : nums) {
            cnt[x]++;
        }
        
        vector<int> ans;
        // priority_queue默认即为大顶堆。
        priority_queue<pair<int, int>> pq;
        for (auto kv : cnt) {
            pq.push(make_pair(kv.second, kv.first));
            if (pq.size() > cnt.size() - k) {
                ans.push_back(pq.top().second);
                pq.pop();
            }
        }
        return ans;
    }
};
```

### 3、Solution-3：Bucket Sort
1. 统计每个数字出现的次数，使用一个哈希表存起来即可，时间复杂度O(n)；假设大小为m；

2. 对于大小为n的nums来说，单个元素最多出现n次。

3. 构造一个大小为n + 1的桶bucket。bucket[i]是一个一维数组，存放所有出现i次的元素。

4. 从后往前读取bucket，当读取的元素个数达到k时，就停止。

5. Code(https://leetcode.com/submissions/detail/324093410/)
AC
时间复杂度：O(n) 
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        int n = nums.size();
        if (n == 0) {
            return {};
        }
        
        vector<int> ans;
        unordered_map<int, int> cnt; // num to frequency
        for (int x : nums) {
            cnt[x]++;
        }
        
        vector<vector<int>> bucket(n + 1);
        for (auto kv : cnt) {
            bucket[kv.second].push_back(kv.first);
        }
        
        for (int i = n; i >= 1; i--) {
            for (int x : bucket[i]) {
                ans.push_back(x);
                k--;
            }
            if (k == 0) {
                break;
            }
        }
        
        return ans;
    }
};
```

