---
title: Leetcode-208.Implement Trie (Prefix Tree)
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- String
- Trie
- Design
---

## 一、题意

### 0、题目链接
[208.Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

### 1、Description
【输入】
无；
【输出】
设计一个类Trie，实现字典树的以下操作：
insert(string s)：将字符串s插入字典树中，保存起来；
search(string s)：判断字典树中是否存在字符串s，返回true/false；
startWith(string s)：判断字典树中是否存在以s为前缀的字符串，返回true/false；
【要求】
无；

### 2、Example
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true

<!-- more -->

### 3、Corner Case
1. 假定所有的字符串中仅包含：小写字母，即'a'~'z'；
2. 假定所有的字符串均非空；

## 二、题解

### 0、思考
1. 字典树，可以在字符串数据集中高效搜索单词；
实际应用：单词自动补全；拼写检查；IP路由（最长前缀匹配）；
哈希表虽然可以以O(1)的时间复杂度搜索，但它不支持：
找到同一前缀的所有单词；
同时，随着哈希表的不断扩大，更容易出现冲突，使得时间复杂度退化为O(n)；

2. 同时，Trie在存储相同前缀的若干个字符串时，可以使用较少的空间；

3. 字典树中，每一个结点需要记录以下两条信息：
第一：当前结点是否为一个字符串的末尾结点；bool isEnd;
第二：当前结点的若干个后继结点的地址；TrieNode* next[26];

4. 注意：不必存储当前结点代表的字符值。
因为：除了root结点外，每个结点cur都代表着一个字符值；
可以根据其前驱结点pre来知道cur实际代表的字符：
若pre->next[i] == cur，说明cur实际代表的字符为：('a' + i)；

5. 字典树在逻辑上，是一棵多叉树。

6. 初始化：为root分配空间；

7. 析构函数：释放字典树所使用的空间；

### 1、Solution-1：Trie with TrieNode
1. 使用TrieNode来辅助实现Trie；

2. Code(https://leetcode.com/submissions/detail/347023943/)
AC
时间复杂度：O(n)-插入n个字符的字符串；O(n)-搜索n个字符的字符串；O(n)-搜索以n个字符为前缀的字符串；
空间复杂度：O(n)-插入n个字符的字符串；O(1)-搜索n个字符的字符串；O(1)-搜索以n个字符为前缀的字符串；
```C++
class TrieNode {
public:
    bool isEnd;
    unordered_map<char, TrieNode*> next;
    TrieNode() {
        isEnd = false;
    }
    ~TrieNode() {
        for (auto kv : next) {
            delete kv.second;
        }
    }
};
 
class Trie {
private:
    TrieNode* root;
public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }
    
    ~Trie() {
        delete root;
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        TrieNode* cur = root;
        for (char c : word) {
            c = c - 'a';
            if (cur->next[c] == NULL) {
                cur->next[c] = new TrieNode();
            }
            cur = cur->next[c];
        }
        cur->isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        TrieNode* cur = root;
        for (char c : word) {
            c = c - 'a';
            if (cur->next[c] == NULL) {
                return false;
            }
            cur = cur->next[c];
        }
        return cur->isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        TrieNode* cur = root;
        for (char c : prefix) {
            c = c - 'a';
            if (cur->next[c] == NULL) {
                return false;
            }
            cur = cur->next[c];
        }
        return true;
    }
};
```

### 2、Solution-2：Trie Using Array or Map
1. 直接用类Trie定义为一个字典树结点；

2. Code(https://leetcode.com/submissions/detail/346182989/)
AC
时间复杂度：O(n)-插入n个字符的字符串；O(n)-搜索n个字符的字符串；O(n)-搜索以n个字符为前缀的字符串；
空间复杂度：O(n)-插入n个字符的字符串；O(1)-搜索n个字符的字符串；O(1)-搜索以n个字符为前缀的字符串；
```C++
class Trie {
private:
    bool isEnd;
    Trie* next[26];
public:
    /** Initialize your data structure here. */
    Trie() {
        isEnd = false;
        for (int i = 0; i <= 26 - 1; i++) {
            next[i] = NULL;
        }
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Trie* cur = this;
        for (char c : word) {
            c = c - 'a';
            if (cur->next[c] == NULL) {
                cur->next[c] = new Trie();
            }
            cur = cur->next[c];
        }
        cur->isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Trie* cur = this;
        for (char c : word) {
            c = c - 'a';
            if (cur->next[c] == NULL) {
                return false;
            }
            cur = cur->next[c];
        }
        return cur->isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Trie* cur = this;
        for (char c : prefix) {
            c = c - 'a';
            if (cur->next[c] == NULL) {
                return false;
            }
            cur = cur->next[c];
        }
        return true;
    }
};
```

3. Code(https://leetcode.com/submissions/detail/346247263/)
AC
时间复杂度：O(n)-插入n个字符的字符串；O(n)-搜索n个字符的字符串；O(n)-搜索以n个字符为前缀的字符串；
空间复杂度：O(n)-插入n个字符的字符串；O(1)-搜索n个字符的字符串；O(1)-搜索以n个字符为前缀的字符串；
```C++
class Trie {
private:
    bool isEnd;
    unordered_map<char, Trie*> m;
public:
    /** Initialize your data structure here. */
    Trie() {
        isEnd = false;
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Trie* cur = this;
        for (char c : word) {
            if (cur->m.count(c) == 0) {
                cur->m[c] = new Trie();
            }
            cur = cur->m[c];
        }
        cur->isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Trie* cur = this;
        for (char c : word) {
            if (cur->m.count(c) == 0) {
                return false;
            }
            cur = cur->m[c];
        }
        return cur->isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Trie* cur = this;
        for (char c : prefix) {
            if (cur->m.count(c) == 0) {
                return false;
            }
            cur = cur->m[c];
        }
        return true;
    }
};
```

4. 总结
形式1：先实现类TrieNode，再实现类Trie；
形式2：直接实现类Trie；
成员变量1：bool isEnd；
成员变量2：Trie* next[26]；
其中根结点不代表任何字符，其余结点均代表一个字符；