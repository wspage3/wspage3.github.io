---
title: Leetcode-451.Sort Characters By Frequency
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
[451.Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

### 1、Description
【输入】
给定一个字符串s，长度为n；
【输出】
按照每个字母出现的频率，对s进行排序：频率高的在前、低的在后；
【要求】
如果两个字母的频率相同，则不要求特定顺序，但相同字母需要连续；

### 2、Example
Input:
"cccaaa"
Output:
"cccaaa"
Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.

<!-- more -->

### 3、Corner Case
1. n == 0时，无需处理，返回s即可；
2. n == 1时，无需处理，返回s即可；
3. 区分大小写，'a'和'A'是两个不同的字母；

## 二、题解

### 0、思考
1. 类似于Top k问题；

### 1、Solution-1：Max Heap
1. 统计每个字母出现的次数，使用一个哈希表存起来即可，时间复杂度O(n)；假设大小为m；

2. 维护一个大小为m的大顶堆；

3. 将哈希表中的m个数据插入大顶堆中。循环结束后，依次取出堆顶元素即可。

4. Code(https://leetcode.com/submissions/detail/324616117/)
AC
时间复杂度：O(m * log m)  
空间复杂度：O(n)
```C++
class Solution {
public:
    string frequencySort(string s) {
        int n = s.size();
        if (n == 0) {
            return "";
        }
        
        unordered_map<char, int> cnt;
        for (char c : s) {
            cnt[c]++;
        }
        
        priority_queue<pair<int, char>> pq;
        for (auto kv : cnt) {
            pq.push(make_pair(kv.second, kv.first));
        }
        
        string ans;
        while (!pq.empty()) {
            for (int num = 1; num <= cnt[pq.top().second]; num++) {
                ans.push_back(pq.top().second);
            }
            pq.pop();
        }
        return ans;
    }
};
```

### 2、Solution-2：Bucket Sort
1. 统计每个字母出现的次数，使用一个哈希表存起来即可，时间复杂度O(n)；假设大小为m；

2. 对于大小为n的s来说，单个字母最多出现n次。

3. 构造一个大小为n + 1的桶bucket。bucket[i]是一个一维数组，存放所有出现i次的字母。

4. 从后往前读取bucket。

5. Code(https://leetcode.com/submissions/detail/324712398/)
AC
时间复杂度：O(n) 
空间复杂度：O(n)
```C++
class Solution {
public:
    string frequencySort(string s) {
        int n = s.size();
        if (n == 0) {
            return "";
        }
        
        unordered_map<char, int> cnt;
        for (char c : s) {
            cnt[c]++;
        }
        
        vector<vector<char>> buckets(n + 1);
        for (auto kv : cnt) {
            buckets[kv.second].push_back(kv.first);
        }
        
        string ans;
        for (int i = n; i >= 1; i--) {
            int size = buckets[i].size();
            for (int j = 0; j <= size - 1; j++) {
                for (int num = 1; num <= i; num++) {
                    ans.push_back(buckets[i][j]);
                }
            }
        }
        return ans;
    }
};
```

