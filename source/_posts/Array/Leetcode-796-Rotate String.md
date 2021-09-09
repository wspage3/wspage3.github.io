---
title: Leetcode-796.Rotate String
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- String
---

## 一、题意

### 0、题目链接
[796.Rotate String](https://leetcode.com/problems/rotate-string/)

### 1、Description
【输入】
给定两个字符串，A和B，长度分别为n和m；
【输出】
判断A是否可以由B经过若干次“旋转”得到；
“旋转”：将字符串最后一个字母移到字符串的开头；
【要求】
无；

### 2、Example
Input: A = 'abcde', B = 'cdeab'
Output: true

Input: A = 'abcde', B = 'abced'
Output: false

<!-- more -->

### 3、Corner Case
1. n != m时，直接返回false；
2. n = m = 0时，返回true；
3. 考虑n、m相等，且均大于等于1的情况；

## 二、题解

### 0、思考
1. 类似于189.Rotate Array。“旋转”的定义完全一致；

2. 189题是对array进行k次“旋转”，得出答案；
本题是对string进行若干次“旋转”，然后判断；

### 1、Solution-1：Brute Force
1. 由于旋转k次，与旋转k % n次，得到的字符串是相同的。

2. 所以枚举所有的可能，即对A旋转：0、1、2...n - 1次。每次得到的结果与B比较。

3. string构造函数：string(s, start, len)；
从字符串s的start位置开始，取len个字符。若len == 0，则构造结果为空字符串；
时间复杂度：与len成线性；

4. Code(https://leetcode.com/submissions/detail/337175728/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool rotateString(string A, string B) {
        int n = A.size();
        int m = B.size();
        if (n != m) {
            return false;
        }
        if (n == 0) {
            return true;
        }
        
        for (int i = 0; i <= n - 1; i++) {
            string left(A, n - i, i);
            string right(A, 0, n - i);
            string temp(left + right);
            if (temp == B) {
                return true;
            }
        }
        
        return false;
    }
};
```

### 2、Solution-2：Using std::string::find()
1. 将本问题转换为字符串匹配问题；

2. 以abcde为例，观察：
rotate 0次：abcde；包含在{**abcde**}abcde中；
rotate 1次：eabcd；包含在abcd{**eabcd**}e中；
rotate 2次：deabc；包含在abc{**deabc**}de中；
rotate 3次：cdeab；包含在ab{**cdeab**}cde中；
rotate 4次：bcdea；包含在a{**bcdea**}bcde中；

3. std::string::find(target)：找到target的首字符位置。
找不到，返回string::npos；
时间复杂度：O(n * n)。注：此处find的实现应该是朴素的，而非KMP等。（待考究）

4. 进一步优化：使用KMP等字符串匹配算法，使得时间复杂度降低到O(2*n + n) = O(n)；

3. Code(https://leetcode.com/submissions/detail/282420452/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool rotateString(string A, string B) {
        return A.size() == B.size() && (A + A).find(B) != string::npos;
    }
};
```