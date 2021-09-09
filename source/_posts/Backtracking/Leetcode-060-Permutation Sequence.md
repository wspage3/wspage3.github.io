---
title: Leetcode-060.Permutation Sequence
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Permutation
- Array
---

## 一、题意

### 黑板上排列组合，你舍得解开吗~~

### 0、题目链接
[060.Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)

### 1、Description
【输入】
给定一个正整数n；n在[1, 9]中；
给定一个正整数k；k在[1, n!]中；
【输出】
求出第k个【全排列序列】；此处k从1开始计数；
全排列序列中的元素为[1, n]中的全部元素：每个元素用一次，不重复，不遗漏；
【要求】
无；

### 2、Example
Input: n = 4, k = 9
Output: "2314"

<!-- more -->

### 3、Corner Case
1. n在[1, 9]中；
2. k保证合法：k在[1, n!]中；
3. 当n == 1时，k一定为1，直接返回"1"；

## 二、题解

### 0、思考
1. "排列问题"的四种题型：
给定nums，求满足sum为target的排列的个数；【377】
求nums所有的全排列，并输出；【046】【047】
给定某个全排列序列，求下一个全排列序列；【031】
给定nums，给定正数k，求第k个全排列序列；【060】

2. 观察：
对于n = 4，即nums = [1, 2, 3, 4]来说。全排列个数 = 4! = 24个；其中，
以1开头的个数 = 6 = 3!；
以2开头的个数 = 6 = 3!；
以3开头的个数 = 6 = 3!；
以4开头的个数 = 6 = 3!；

### 1、Solution-1
1. 算法过程：
首先：生成[0, n - 1]的阶乘值，存储在factorial中；
其次：生成nums，从1开始，到n结束；
然后：使得k从0计数：k- -；
最后：依次考虑ans的n个位置上是哪个字符；

2. 举例：n = 4, k = 14
factorial = [1, 1, 2, 6];
nums = [1, 2, 3, 4];
k = 14 - 1 = 13;
i = 0: cur = 13 / 6 = 2，代表第一字符前面有2组，所以为'3'。k -= 2 * 6 = 1；在nums中删除'3'；
i = 1: cur = 1 / 2 = 0，代表第二字符前面有0组，所以为'1'。k -= 0 = 1；在nums中删除'1'；
i = 2: cur = 1 / 1 = 1，代表第三字符前面有1组，所以为'4'。k -= 1 * 1 = 0；在nums中删除'4'；
i = 3: cur = 0 / 1 = 0，代表第四字符前面有0组，所以为'2'。k -= 0 = 0；在nums中删除'2'；
ans = "3142"；

3. Code(https://leetcode.com/submissions/detail/350226338/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    string getPermutation(int n, int k) {
        if (n == 1) {
            return "1";
        }
        
        vector<int> factorial(n, 0);
        factorial[0] = 1;
        for (int i = 1; i <= n - 1; i++) {
            factorial[i] = i * factorial[i - 1];
        }
        // n = 4, factorial = [1, 1, 2, 6]
        
        string dict(n, '1');
        for (int i = 0; i <= n - 1; i++) {
            dict[i] += i;
        }
        // n = 4, dict = [1, 2, 3, 4]
        
        string ans(n, ' ');
        k--;
        for (int i = 0; i <= n - 1; i++) {
            int cur = k / factorial[n - 1 - i];
            ans[i] = dict[cur];
            dict.erase(dict.begin() + cur);
            k = k - cur * factorial[n - 1 - i];
        }
        // n = 4, k = 14, ans = "3142"
        
        return ans;
    }
};
```

