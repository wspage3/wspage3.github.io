---
title: Leetcode-648.Replace Words
categories: 
- [Leetcode]
- [数据结构与算法] 
tags: 
- String
- Trie
- Hash Table
---

## 一、题意

### 0、题目链接
[648.Replace Words](https://leetcode.com/problems/replace-words/)

### 1、Description
【输入】
给定一个长度为n的字符串数组dictionary；
给定一个长度为len的字符串sentence：包含若干个单词，每个单词之间有一个空格，且sentence无前置和后置的空格；
【输出】
对于sentence中的单词s，若s的某个前缀出现在dictionary中，则用这个前缀替换掉s。
注：假设"a","ab"均出现在dictionary中，同时又都是"abc"的前缀，此时用较短的前缀"a"来替换掉"abc"。
返回替换后的sentence。
【要求】
无；

### 2、Example
Input: dictionary = ["catt","cat","bat","rat"], sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 1000；
2. 1 <= len <= 1000000；
3. dictionary中的单词、sentence中的单词，都仅包含小写字母；
4. sentence合法：每两个单词间有且仅有一个空格、无前置后置空格；

## 二、题解

### 1、Solution-1：Straightforward(Hash Table)
1. 将dictionary中的所有单词，依次插入到哈希表m中；

2. 解析sentence中的所有单词，放入字符串数组words中；

3. 考察words中的每个单词，并拼接到ans后面；

4. Code(https://leetcode.com/submissions/detail/426021863/)
AC
n：dictionary中的单词个数
L：dictionary中的单词平均长度
m：sentence中的单词个数
l：sentence中的单词平均长度
其中m * l = len
时间复杂度：O(n * L + m * l * l)
空间复杂度：O(n * L) 
```C++
class Solution {
private:
    void helper(string& s, vector<string>& words) {
        int n = s.size();       
        int i = 0;
        while (i <= n - 1) {
            int j = s.find(' ', i);
            words.push_back(s.substr(i, j - i));
            i = j + 1;
        }
    }
public:
    string replaceWords(vector<string>& dictionary, string sentence) {
        unordered_map<string, bool> m;
        for (string s : dictionary) {
            m[s] = true;
        }
        
        sentence.push_back(' ');
        vector<string> words;
        helper(sentence, words);
        
        string ans;
        for (string word : words) {
            for (int l = 1; l <= word.size(); l++) {
                if (m.count(word.substr(0, l))) {
                    word = word.substr(0, l);
                    break;
                }
            }
            ans += word + " ";
        }
        // 题目保证了words非空，此处pop_back()没问题。
        ans.pop_back();
        return ans;
    }
};
```

### 2、Solution-2：Trie
1. 将dictionary中的所有单词，依次插入到Trie中；

2. 解析sentence中的所有单词，放入字符串数组words中；

3. 考察words中的每个单词，并拼接到ans后面；

4. 对insert进行优化：
由于我们找较短的前缀，所以当insert的时候，可以优化。
例如：在进行insert("abc")的时候，若"ab"已经在字典树中，那么就不将"abc"插入字典树中了。

5. Code(https://leetcode.com/submissions/detail/426001823/)
AC
n：dictionary中的单词个数
L：dictionary中的单词平均长度
m：sentence中的单词个数
l：sentence中的单词平均长度
其中m * l = len
时间复杂度：O(n * L + m * l)
空间复杂度：O(n * L) 
```C++
class Trie {
public:
    bool isEnd;
    Trie* next[26];
    Trie() {
        isEnd = false;
        for (int i = 0; i <= 25; i++) {
            next[i] = NULL;
        }
    }
    // 对insert优化：
    // 当s的某个前缀s[0, i]已经出现在字典中的时候，不再将s插入到字典中；
    void insert(string s) {
        Trie* cur = this;
        for (char c : s) {
            c -= 'a';
            if (cur->next[c] == NULL) {
                cur->next[c] = new Trie();
            }
            // 对insert优化：
            if (cur->next[c] != NULL && cur->next[c]->isEnd == true) {
                return;
            }
            cur = cur->next[c];
        }
        cur->isEnd = true;
    }
    string find(string s) {
        Trie* cur = this;
        int idx = -1;
        int n = s.size();
        for (int i = 0; i <= n - 1; i++) {
            char c = s[i];
            c -= 'a';
            if (cur->next[c] == NULL) {
                // 当前字符匹配不到。
                // 说明s[0, i]这个前缀在字典树中不存在。
                break;
            } else if (cur->next[c] != NULL && cur->next[c]->isEnd == true) {
                // 当前字符匹配到了，且是某个单词的尾字符。
                // 说明s[0, i]这个前缀在字典树中存在。
                idx = i;
                break;
            } else {
                // 当前字符匹配到了，但不是某个单词的尾字符。
                // 继续向下查找。
                cur = cur->next[c];
            }
        }
        return idx == -1 ? s : s.substr(0, idx + 1);
    }
};
 
class Solution {
private:
    void helper(string& s, vector<string>& words) {
        int n = s.size();       
        int i = 0;
        while (i <= n - 1) {
            int j = s.find(' ', i);
            words.push_back(s.substr(i, j - i));
            i = j + 1;
        }
    }
public:
    string replaceWords(vector<string>& dictionary, string sentence) {
        Trie* trie = new Trie();
        for (string s : dictionary) {
            trie->insert(s);
        }
        
        sentence.push_back(' ');
        vector<string> words;
        helper(sentence, words);
        
        string ans;
        for (string word : words) {
            ans += trie->find(word) + " ";
        }
        // 题目保证了words非空，此处pop_back()没问题。
        ans.pop_back();
        return ans;
    }
};
```