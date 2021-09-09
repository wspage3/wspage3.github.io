---
title: Leetcode-792.Number of Matching Subsequences
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Two Pointers
- Hash Table
---

## 一、题意

### 0、题目链接
[792.Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/)

### 1、Description
【输入】
给定一个字符串s，长度为ns；
给定一个长度为n的字符串数组words；
【输出】
求words中满足条件的元素个数；
满足条件：words[i]是s的子序列；
【要求】
无；

### 2、Example
Input: 
S = "abcde"
words = ["a", "bb", "acd", "ace"]
Output: 3
Explanation: There are three words in words that are a subsequence of S: "a", "acd", "ace".

<!-- more -->

### 3、Corner Case
1. 1 <= ns <= 50000
2. 1 <= n <= 50
3. 1 <= words[i].size() <= 5000
4. 所有字符串均只包含小写字母；

## 二、题解

### 1、Solution-1：Two Pointers
1. Leetcode-392-Is Subsequence
我们以O(nt)的时间复杂度，判断s是否为t的子序列；

2. 本题，直接考虑每个words[i]；

3. Code(https://leetcode.com/submissions/detail/431541423/)
AC
时间复杂度：O(n * nt)
空间复杂度：O(1)
```C++
class Solution {
private:
    int isSubsequence(string& word, string& s) {
        int ns = s.size();
        int n = word.size();
        if (n > ns) {
            return 0;
        }
        
        int idx = 0;
        for (char c : word) {
            idx = s.find(c, idx);
            if (idx == string::npos) {
                return 0;
            }
            idx++;
        }
        return 1;
    }
public:
    int numMatchingSubseq(string s, vector<string>& words) {
        int ans = 0;
        for (string word : words) {
            ans += isSubsequence(word, s);
        }
        return ans;
    }
};
```

4. 使用哈希表优化：若words中有重复字符串，则不进行重复判断子序列；

5. Code(https://leetcode.com/submissions/detail/431550358/)
AC
dup：words中重复字符串的个数
len：words中重复字符串的平均长度
时间复杂度：O((n - dup) * nt)
空间复杂度：O(dup * len)
```C++
class Solution {
private:
    int isSubsequence(string word, string& s) {
        int ns = s.size();
        int n = word.size();
        if (n > ns) {
            return 0;
        }
        
        int idx = 0;
        for (char c : word) {
            idx = s.find(c, idx);
            if (idx == string::npos) {
                return 0;
            }
            idx++;
        }
        return 1;
    }
public:
    int numMatchingSubseq(string s, vector<string>& words) {
        unordered_map<string, int> m;
        for (string word : words) {
            m[word]++;
        }
        
        int ans = 0;
        for (auto kv : m) {
            ans += isSubsequence(kv.first, s) * kv.second;
        }
        return ans;
    }
};
```