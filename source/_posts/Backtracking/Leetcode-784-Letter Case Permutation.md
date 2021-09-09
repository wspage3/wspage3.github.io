---
title: Leetcode-784.Letter Case Permutation
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
- BFS
- Queue
- Bit Manipulation
- Permutation
---

## 一、题意

### 0、题目链接
[784.Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
s性质：元素都是数字或者大小写字母；
【输出】
若考虑字母的大小写，求s所有的表示方式；
答案用一维数组表示；
【要求】
无；

### 2、Example
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 12；

## 二、题解

### 0、思考
1. 隐式树的遍历；

### 1、Solution-1：Backtracking + Bit Manipulation
1. 假定s的长度为n；

2. 从0开始，到n - 1结束，依次考虑每个位置上的字符；

3. 对于i来说：
若s[i]是字符，则有两种可能；选择一种，进入下一层；

4. 结束条件：所有的位置都考虑过了；

5. Code(https://leetcode.com/submissions/detail/423222644/)
AC
// 每个结点的“度数”：2
// 递归树的高度为：n
时间复杂度：O(2 ^ n)
空间复杂度：O(n)
```C++
class Solution {
private:
    void backtracking(int i, string& S, string& sub, vector<string>& ans) {
        int n = S.size();
        if (i == n) {
            ans.push_back(sub);
            return;
        }
        
        sub.push_back(S[i]);
        backtracking(i + 1, S, sub, ans);
        sub.pop_back();
        
        if (isalpha(S[i])) {
            sub.push_back(S[i] ^ 32);
            backtracking(i + 1, S, sub, ans);
            sub.pop_back();
        }
    }
public:
    vector<string> letterCasePermutation(string S) {
        string sub;
        vector<string> ans;
        backtracking(0, S, sub, ans);
        return ans;
    }
};
```

6. 位运算技巧
a的ASCII码为97，A的ASCII码为65；
97 ^ 32 = 65
65 ^ 32 = 97

### 2、Solution-2：BFS Using Queue + Bit Manipulation
1. 举例：s = "abc"
初始化：将"abc"入队；
i = 0：[abc, Abc]
i = 1：[abc, aBc, Abc, ABc]
i = 2：[abc, abC, aBc, aBC, Abc, AbC, ABc, ABC]
将队列中的全部字符串作为答案即可；

2. Code(https://leetcode.com/submissions/detail/423230023/)
AC
// 每个结点的“度数”：2
// 递归树的高度为：n
时间复杂度：O(2 ^ n)
空间复杂度：O(2 ^ n) // 空间复杂度较差；
```C++
class Solution {
public:
    vector<string> letterCasePermutation(string S) {
        queue<string> q;
        q.push(S);
        
        int n = S.size();
        for (int i = 0; i <= n - 1; i++) {
            if (isdigit(S[i])) {
                continue;
            }
            int size = q.size();
            for (int num = 1; num <= size; num++) {
                string cur = q.front();
                q.pop();
                q.push(cur);
                cur[i] = cur[i] ^ 32;
                q.push(cur);
            }
        }
        
        vector<string> ans;
        while (!q.empty()) {
            ans.push_back(q.front());
            q.pop();
        }
        return ans;
    }
};
```