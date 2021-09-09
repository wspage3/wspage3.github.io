---
title: Leetcode-692.Top K Frequent Words
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
[692.Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

### 1、Description
【输入】
给定一个非空数组words，大小为n；每个元素都是一个字符串；
给定一个正整数k；
【输出】
将words中出现最频繁的前k个字符串，以数组的形式返回；
【要求】
对返回的k个字符串，要求频率高的在前，频率低的在后；如果频率相等，字典序低的在前，字典序高的在后；

### 2、Example
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.

<!-- more -->

### 3、Corner Case
1. n >= 1
2. k合法：1 <= k <= number of unique elements
3. 字符串中仅包含小写字母

## 二、题解

### 0、思考
1. 类似于347.Top K Frequent Elements；
2. 347是求出频率最高的k个数字，不要求其顺序，题目数据保证了对于一个特定的k，答案是唯一的；
3. 本题是求出频率最高的k个字符串，同时要求按照一定的顺序：频率高的在前、字典序小的在前；

### 1、Solution-1：Min Heap
1. 统计每个字符串出现的次数，使用一个哈希表存起来即可，时间复杂度O(n)；假设大小为m；

2. 维护一个大小为k的小顶堆；

3. 将哈希表中的m个数据插入小顶堆中。若堆大小超过了k，则把堆顶元素去除。循环结束后，留下的就是Top K的字符串了。

4. Code(https://leetcode.com/submissions/detail/324439519/)
AC
时间复杂度：O(n * log k)  
空间复杂度：O(n)
```C++
class Solution {
private:
    struct myGreater {
        bool operator() (const pair<int, string>& a, const pair<int, string>& b) {
            // 1.首先考虑频率。频率大的放在队尾。（更不容易被挤出去）
            // 2.其次考虑字母序。当频率相同时，字母序小的放在队尾。（更不容易被挤出去）
            // 若本函数返回true，则在队列中，a处于更尾端。
            return a.first > b.first || (a.first == b.first && a.second < b.second);
        }
    };
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        int n = words.size();
        if (n == 0) {
            return {};
        }
        
        unordered_map<string, int> cnt; // word to frequency
        for (string word : words) {
            cnt[word]++;
        }
        
        priority_queue<pair<int, string>, vector<pair<int, string>>, myGreater> pq;
        for (auto kv : cnt) {
            pq.push(make_pair(kv.second, kv.first));
            if (pq.size() > k) {
                pq.pop();
            }
        }
        
        vector<string> ans;
        while (!pq.empty()) {
            ans.push_back(pq.top().second);
            pq.pop();
        }
        
        // 逆序输出：将频率大的放在ans的最前面。
        reverse(ans.begin(), ans.end());
        
        return ans;
    }
};
```

5. Code(https://leetcode.com/submissions/detail/324583349/)
AC
时间复杂度：O(n * log k)  
空间复杂度：O(n)
```C++
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        int n = words.size();
        if (n == 0) {
            return {};
        }
        
        unordered_map<string, int> cnt; // word to frequency
        for (string word : words) {
            cnt[word]++;
        }        
        
        // 用 lambda 比较元素。
        auto cmp = [](const pair<int, string>& a, const pair<int, string>& b) { 
            return a.first > b.first || (a.first == b.first && a.second < b.second);
        };
        
        priority_queue<pair<int, string>, vector<pair<int, string>>, decltype(cmp)> pq(cmp);
        for (auto kv : cnt) {
            pq.push(make_pair(kv.second, kv.first));
            if (pq.size() > k) {
                pq.pop();
            }
        }
        
        vector<string> ans;
        while (!pq.empty()) {
            ans.push_back(pq.top().second);
            pq.pop();
        }
        
        // 逆序输出：将频率大的放在ans的最前面。
        reverse(ans.begin(), ans.end());
        
        return ans;
    }
};
```

### 2、Solution-2：Max Heap
1. 本题不能像347一样，使用大顶堆。
因为：当堆的大小超过m - k时，需要将堆顶元素挤出去。
而挤出去的顺序不能保证满足题意：即频率高的在最前面；当频率相同时，字典序小的在前面。


### 3、Solution-3：Bucket Sort
1. 统计每个字符串出现的次数，使用一个哈希表存起来即可，时间复杂度O(n)；假设大小为m；

2. 对于大小为n的words来说，单个元素最多出现n次。

3. 构造一个大小为n + 1的桶bucket。bucket[i]是一个一维数组，存放所有出现i次的字符串。

4. 对每个bucket，需要进行排序：因为对于频率相同的，需要先返回字典序小的字符串。

4. 极端情况下，每个单词仅仅出现一次。桶1内有n个元素，对其进行排序需要O(n * log n)，整体时间复杂度为O(n * log n)，非最优解。