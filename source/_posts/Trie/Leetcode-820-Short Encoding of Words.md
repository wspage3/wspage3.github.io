---
title: Leetcode-820.Short Encoding of Words
categories: 
- [Leetcode]
- [数据结构与算法] 
tags: 
- String
- Trie
---

## 一、题意

### 0、题目链接
[820.Short Encoding of Words](https://leetcode.com/problems/short-encoding-of-words/)

### 1、Description
【输入】
给定一个长度为n的字符串数组words；
【输出】
对words进行编码：可以得到一个字符串s和一个索引数组indexes（和words一一对应）；
举例：见如下Example；
求出编码后，s的最短长度是多少；
【要求】
无；

### 2、Example
Input: words = ["time", "me", "bell"]
Output: 10
Explanation: S = "time#bell#" and indexes = [0, 2, 5].

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 2000；
2. words[i]中仅包含小写字母；

## 二、题解

### 0、思考
1. 若s0是s1的后缀，则s1编码后，s0就无需编码了，找出其对应的索引即可；

2. Trie解决前缀相关的问题；
在本题中，可以将word逆序，将处理后缀的问题转化为处理前缀；

### 1、Solution-1：Trie
1. 将words中的所有单词，逆序后，依次插入到Trie中；

2. 插入时，记录每个单词最后一个字符对应的结点的深度，保存到leaves数组中；

3. 计算s的长度：所有叶结点的深度之和。
leaves数组中的元素不一定都是叶结点，需要判断一下；
例如：先添加"abc"，后添加"abcd"；

4. 去重：当words中有重复单词时：可以通过哈希表去重后再insert，也可以在insert的时候判断；

5. 举个例子：
words = ["time", "me", "bell", "tme"]
插入到Trie之后：叶结点有t,t,b，m不是叶结点。
其深度为：5,4,5，所以最后的答案就是14。
s = "time#bell#tme#"
indexes = [0, 2, 5, 10]

5. Code(https://leetcode.com/submissions/detail/426105016/)
AC
n：words中的单词个数
L：words中的单词平均长度
时间复杂度：O(n * L + n * 26)
空间复杂度：O(n * L) 
```C++
class Trie {
public:
    bool isEnd;
    Trie* next[26];
    vector<pair<Trie*, int>> leaves;
    Trie() {
        isEnd = false;
        for (int i = 0; i <= 25; i++) {
            next[i] = NULL;
        }
    }
    void insert(string word) {
        Trie* cur = this;
        for (char c : word) {
            c -= 'a';
            if (cur->next[c] == NULL) {
                cur->next[c] = new Trie();
            }
            cur = cur->next[c];
        }
        if (cur->isEnd) {
            return;
        }
        cur->isEnd = true;
        leaves.push_back(make_pair(cur, word.size()));
    }
    bool isLeaf(Trie* root) {
        if (root == NULL) {
            return false;
        }
        for (int i = 0; i <= 25; i++) {
            if (root->next[i] != NULL) {
                return false;
            }
        }
        return true;
    }
    int getAns() {
        int ans = 0;
        for (auto leaf : leaves) {
            if (isLeaf(leaf.first)) {
                ans += leaf.second + 1;
            }
        }
        return ans;
    }
};

class Solution {
public:
    int minimumLengthEncoding(vector<string>& words) {
        Trie* trie = new Trie();
        
        for (string word : words) {
            string s = word;
            reverse(s.begin(), s.end());
            trie->insert(s);
        }
        return trie->getAns();
    }
};
```