---
title: Leetcode-387.First Unique Character in a String
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- String
- Hash Table
---

## 一、题意

### 0、题目链接
[387.First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

### 1、Description
【输入】
给定一个长度为n的字符串；
【输出】
找出第一个只出现一次的字母，返回其索引；如果没有只出现一次的字母，则返回-1；
【要求】
无；

### 2、Example
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.

<!-- more -->

### 3、Corner Case
1. 字符串中仅包含小写字母；
2. 当n == 0时，返回-1；
3. 当n == 1时，返回0；

## 二、题解

### 1、Solution-1：Brute Force(Two-round)
1. 在区间[0, n - 1]遍历字符串，哈希表（或长度26的int数组）统计每个字母出现的次数。

2. 再次在区间[0, n - 1]遍历字符串，若字母s[i]出现的次数为1，返回i即可；如果没找到，返回-1；

3. 缺点：需要遍历两次字符串。当字符串长度非常大时，较耗时。

4. Code(https://leetcode.com/submissions/detail/324736338/)
AC
时间复杂度：O(n)
空间复杂度：O(26) = O(1)
```C++
class Solution {
public:
    int firstUniqChar(string s) {
        int n = s.size();
        if (n == 0) {
            return -1;
        }
        if (n == 1) {
            return 0;
        }
        int cnt[26] = {0};
        for (char c : s) {
            cnt[c - 'a']++;
        }
        for (int i = 0; i <= n - 1; i++) {
            if (cnt[s[i] - 'a'] == 1) {
                return i;
            }
        }
        return -1;
    }
};
```

### 2、Solution-2：Brute Force(One-round)
1. 在区间[0, n - 1]遍历字符串，统计每个字母出现的次数，同时记录其最后一次出现的index。

2. 不再对字符串进行遍历。而对我们已经统计好的数据进行遍历。

3. 优点：遍历一次字符串即可。

4. Code(https://leetcode.com/submissions/detail/324962903/)
AC
时间复杂度：O(n)
空间复杂度：O(26) = O(1)
```C++
class Solution {
public:
    int firstUniqChar(string s) {
        int n = s.size();
        if (n == 0) {
            return -1;
        }
        if (n == 1) {
            return 0;
        }
        // Solution-2: traverse the string only once
        int cnt[26][2] = {0};
        for (int i = 0; i <= n - 1; i++) {
            cnt[s[i] - 'a'][0]++;
            cnt[s[i] - 'a'][1] = i;
        }
        int ans = INT_MAX;
        for (int i = 0; i <= 25; i++) {
            if (cnt[i][0] == 1) {
                ans = min(ans, cnt[i][1]);
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

### 3、Solution-3：Brute Force(One-round)
1. 在区间[0, n - 1]遍历字符串，哈希表记录每个字母出现的index。

2. 如果字母c已经出现过了，将其对应的value置为INT_MAX。

3. 如果字母c还没出现过，将其对应的value置为当前的索引。

4. 最后遍历哈希表，找出最小的value，即答案；若没有找到，说明每个字母都出现过不止一次。

5. 优点：遍历一次字符串即可。

6. Code(https://leetcode.com/submissions/detail/324966336/)
AC
时间复杂度：O(n)
空间复杂度：O(26) = O(1)
```C++
class Solution {
public:
    int firstUniqChar(string s) {
        int n = s.size();
        if (n == 0) {
            return -1;
        }
        if (n == 1) {
            return 0;
        }
        // Solution-3: traverse the string only once
        unordered_map<char, int> m;
        for (int i = 0; i <= n - 1; i++) {
            if (m.count(s[i]) == 0) {
                m[s[i]] = i; //如果还没出现过，将该字母对应的value设置为当前idx。
            } else {
                m[s[i]] = INT_MAX; //如果之前已经出现过了，则该字母不可能成为答案，将其value设置为INT_MAX。
            }
        }
        int ans = INT_MAX;
        for (auto kv : m) {
            ans = min(ans, kv.second);
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```