---
title: Leetcode-844.Backspace String Compare
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- String
- Stack
- Two Pointers
---

## 一、题意

### 0、题目链接
[844.Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)

### 1、Description
【输入】
给定两个字符串S、T，长度分别为lenS、lenT；
两个字符串中都仅包含小写字母和'#'，其中'#'代表回退键Backspace。
【输出】
判断两个字符串在处理后，是否相同。
处理：模拟将字符串输入到一个文本编辑器中。
注意：当回退键Backspace前面没有字符时，此时再输入回退键Backspace，文本仍然为空。
【要求】
时间复杂度：O(N) 其中N = lenS + lenT
空间复杂度：O(1)

### 2、Example
Input: S = "ab#c", T = "ad#c"
Output: true
Explanation: Both S and T become "ac".

Input: S = "a##c", T = "#a#c"
Output: true
Explanation: Both S and T become "c".

<!-- more -->

### 3、Corner Case
1. lenS、lenT均大于0
2. 两个字符串中都仅包含小写字母和'#'

## 二、题解

### 1、Solution-1：Brute Force
1. 模拟：将字符串S、T分别转化为处理后的字符串s、t，然后再判断是否相等即可。

2. 处理：
当前char是'#'：如果前面有字符，将其删除即可；如果前面没有字符，不做处理；
当前char不是'#'：将当前字符添加到结尾即可；

3. 使用stack来进行模拟：因为我们只会对字符串的末尾进行添加或者删除操作，与stack的逻辑一致；

4. Code(https://leetcode.com/submissions/detail/331622732/)
AC
时间复杂度：O(N) 其中N = lenS + lenT
空间复杂度：O(N)
```C++
class Solution {
private:
    string convert(string& s) {
        stack<char> stack;
        for (char c : s) {
            if (c == '#') {
                if (!stack.empty()) {
                    stack.pop();
                }
            } else {
                stack.push(c);
            }
        }
        string ans;
        while (!stack.empty()) {
            ans.push_back(stack.top());
            stack.pop();
        }
        // 注意：此时ans得到的是处理后的字符串s的逆序，但不影响两个字符串的比较。
        return ans;
    }
public:
    bool backspaceCompare(string S, string T) {
        string s = convert(S);
        string t = convert(T);
        return s == t;
    }
};
```

### 2、Solution-2：Two Pointers
1. 题目要求以O(1)的空间复杂度来解决，意味着不能保存处理后的结果，再进行比较。而是需要实时逐一对比。

2. 观察：
当我们从前往后遍历一个字符串str时，对于当前字符c，我们并不能保证它一定会保留下来，因为后续可能有Backspace。
当我们从后往前遍历一个字符串str时，对于当前字符c，它一定会保留下来。

3. 基于以上观察：
i遍历S：从lenS - 1开始，到0结束。
j遍历T：从lenT - 1开始，到0结束。
每次各自选一个非'#'的字符进行比较。

4. 若S、T有效长度相等，均为n：则会逐一对比这n个字符是否相等。
如果有一次不匹配，直接跳出循环。此时i、j均大于等于0。
如果第n次也匹配的话：继续循环找下一个i、j。
第n + 1次：各自寻找后，i、j一定都为-1。然后判断：跳出循环。

5. 若S、T有效长度不相等，一个n，一个m，且n > m：则会逐一对比这m个字符是否相等。
如果有一次不匹配，直接跳出循环。此时i、j均大于等于0。
如果第m次也匹配的话：继续循环找下一个i、j。
第m + 1次：此时字符串T已经没有效字符了，j == -1；而S仍有有效字符，i >= 0；然后判断：跳出循环。

6. Code(https://leetcode.com/submissions/detail/331697104/)
AC
时间复杂度：O(N) 其中N = lenS + lenT
空间复杂度：O(1)
```C++
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        int lenS = S.size();
        int lenT = T.size();
        int i = lenS - 1;
        int j = lenT - 1;
        int cntS = 0;
        int cntT = 0;
        while (true) {
            while (i >= 0 && (S[i] == '#' || cntS >= 1)) {
                if (S[i] == '#') {
                    cntS++;
                } else {
                    cntS--;
                }
                i--;
            }
            while (j >= 0 && (T[j] == '#' || cntT >= 1)) {
                if (T[j] == '#') {
                    cntT++;
                } else {
                    cntT--;
                }
                j--;
            }
            if (i >= 0 && j >= 0 && S[i] == T[j]) {
                i--;
                j--;
            } else {
                break;
            }
        }
        return i == -1 && j == -1;
    }
};
```