---
title: Leetcode-211.Add and Search Word - Data structure design
categories: 
- [Leetcode]
- [数据结构与算法] 
tags: 
- String
- Design
- Hash Table
- Trie
- DFS
---

## 一、题意

### 0、题目链接
[211.Add and Search Word - Data structure design](https://leetcode.com/problems/add-and-search-word-data-structure-design/)

### 1、Description
【输入】
无；
【输出】
设计一个类WordDictionary，实现以下操作：
void addWord(word)：将字符串word保存到字典中；
bool search(word)：在字典中搜索字符串word，判断其是否存在；
注意：此处搜索串可以是精确搜索，即严格匹配；也可以是支持'.'的通配符匹配；
【要求】
无；

### 2、Example
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true

<!-- more -->

### 3、Corner Case
1. 假定所有的字符串中仅包含：小写字母，即'a'~'z'；
2. 考虑字符串为空的场景：插入空串、搜索空串；

## 二、题解

### 0、思考
1. 在本题中，通配符'.'可以代表任意一个小写字母，即'a'~'z'；

2. 如果没有通配符搜索的话：可以直接将所有字符串放入哈希表中，插入、搜索时间复杂度均为O(1)；

### 1、Solution-1：Hash Table
1. 按照字符串的长度，将其分批存储。每一批是一个string数组；

2. 搜索长度为len的关键字word的时候，通过哈希表O(1)定位到其批次m[len]，然后再逐一对比；

3. 在v中逐一比较，看word是否可以和v[i]匹配成功。
若某个成功，返回true；若所有都失败，返回false；
最坏O(len)的时间复杂度；

4. Code(https://leetcode.com/submissions/detail/347887880/)
AC
n为字典中字符串的个数；len是字符串平均长度；
时间复杂度：O(1)-插入一个；O(n * len)-搜索一个，最坏情况下，所有字符串的长度都相等，且查找的word为"......"；
空间复杂度：O(n * len)-存储所有；
```C++
class WordDictionary {
private:
    unordered_map<int, vector<string>> m;
    bool isSame(string& s1, string& s2) {
        int len = s1.size();
        for (int i = 0; i <= len - 1; i++) {
            if (s2[i] == '.') {
                continue;
            }
            if (s1[i] != s2[i]) {
                return false;
            }
        }
        return true;
    }
public:
    /** Initialize your data structure here. */
    WordDictionary() {
        
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
        int n = word.size();
        m[n].push_back(word);
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word) {
        int n = word.size();
        for (string s : m[n]) {
            if (isSame(s, word)) {
                return true;
            }
        }
        return false;
    }
};
```

### 2、Solution-2：Trie + DFS
1. 插入和Trie的插入完全一致；

2. 当搜索的关键字中未出现通配符'.'的时候，搜索和Trie的搜索完全一致；

3. 当搜索的关键字中出现通配符的时候：开始进行DFS搜索；

4. Code(https://leetcode.com/submissions/detail/425738714/)
AC
n为字典中字符串的个数；len是字符串平均长度；
时间复杂度：O(len)-插入一个；O(26 ^ len)-搜索一个；
空间复杂度：O(n * len)-存储所有；最坏情况下，n个字符串都没有前缀。
```C++
class WordDictionary {
private:
    bool isEnd;
    WordDictionary* next[26];
    // 以node作为字典树的root结点，搜索word的子串[pos, len - 1]是否在字典中出现。
    bool mySearch(string& word, int pos, WordDictionary* node) {
        int len = word.size();
        // 当word的最后一个字符是'.'时，则执行到此处；
        if (pos == len) {
            return node->isEnd;
        }
        
        WordDictionary* cur = node;
        for (int i = pos; i <= len - 1; i++) {
            char c = word[i];
            if (c == '.') {
                for (int j = 0; j <= 25; j++) {
                    if (cur->next[j] != NULL && mySearch(word, i + 1, cur->next[j]) == true) {
                        return true;
                    }
                }
                return false;
            } else {
                c -= 'a';
                if (cur->next[c] == NULL) {
                    return false;
                }
                cur = cur->next[c];
            }
        }
        // 当word的最后一个字符不是'.'时，则执行到此处；
        return cur->isEnd;
    }
public:
    /** Initialize your data structure here. */
    WordDictionary() {
        isEnd = false;
        for (int i = 0; i <= 25; i++) {
            next[i] = NULL;
        }
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
        WordDictionary* cur = this;
        for (char c : word) {
            c -= 'a';
            if (cur->next[c] == NULL) {
                cur->next[c] = new WordDictionary();
            }
            cur = cur->next[c];
        }
        cur->isEnd = true;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word) {
        WordDictionary* cur = this;
        return mySearch(word, 0, cur);
    }
};
```