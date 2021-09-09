---
title: Leetcode-383.Ransom Note
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Hash Table
---

## 一、题意

### 0、题目链接
[383.Ransom Note](https://leetcode.com/problems/ransom-note/)

### 1、Description
【输入】
给定两个字符串，ransomNote、magazine；长度分别为m、n；
【输出】
判断ransomNote是否可以由magazine构造出来。每个字母仅能使用一次。
【要求】
无；

### 2、Example
Input: ransomNote = "aa", magazine = "aab"
Output: true

<!-- more -->

### 3、Corner Case
1. 两个字符串中均只包含小写字母；
2. 若m > n，直接返回false；

## 二、题解

### 1、Solution-1：Hash Table
1. 遍历magazine，将出现的字母的对应的个数存在哈希表中。

2. 遍历ransomNote，对于字符c，使得cnt[c - 'a']- -。若自减后发现为负数，直接返回false。

3. Code(https://leetcode.com/submissions/detail/342095568/)
AC
时间复杂度：O(m + n)
空间复杂度：O(26)
```C++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int cnt[26] = {};
        int m = ransomNote.size();
        int n = magazine.size();
        if (m > n) {
            return false;
        }
        for (char c : magazine) {
            cnt[c - 'a']++;
        }
        for (char c : ransomNote) {
            cnt[c - 'a']--;。
            if (cnt[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

