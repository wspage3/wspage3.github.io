---
title: Leetcode-771.Jewels and Stones
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Hash Table
---

## 一、题意

### 0、题目链接
[771.Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/)

### 1、Description
【输入】
给定两个字符串，J、S；
J代表：所有宝石的种类；J中所有元素都是不重复的；
S代表：你拥有的所有石头；
【输出】
计算出你拥有的石头中，有几个是宝石；
【要求】
无；

### 2、Example
Input: J = "aA", S = "aAAbbbb"
Output: 3

<!-- more -->

### 3、Corner Case
1. 大小写敏感；'a'和'A'不同；
2. 若lenJ == 0，直接返回0；
3. 若lenS == 0，直接返回0；

## 二、题解

### 1、Solution-1：Hash Table
1. 遍历J，将所有宝石的种类存在哈希表中。

2. 遍历S，判断s[i]是否是宝石。

3. Code(https://leetcode.com/submissions/detail/342091035/)
AC
时间复杂度：O(lenJ + lenS)
空间复杂度：O(lenJ)
```C++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        if (J.size() == 0) {
            return 0;
        }
        
        unordered_map<char, bool> m;
        for (char c : J) {
            m[c] = true;
        }
        
        int cnt = 0;
        int n = S.size();
        for (int i = 0; i <= n - 1; i++) {
            if (m.count(S[i]) == 1) {
                cnt++;
            }
        }
        return cnt;
    }
};
```

