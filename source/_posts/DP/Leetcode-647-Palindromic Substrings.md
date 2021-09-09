---
title: Leetcode-647.Palindromic Substrings
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
- Two Pointers
---

## 一、题意

### 0、题目链接
[647.Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
【输出】
求出s的所有子串中，是回文串的个数；
【要求】
无；

### 2、Example
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".

Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

<!-- more -->

### 3、Corner Case
1. 0 <= n <= 1000
2. n == 0时，输出0；
3. n == 1时，输出1；

## 二、题解

### 0、思考
1. Leetcode-005-Longest Palindromic Substring
求s的所有子串中，最长的回文子串的长度；

2. 本题，求s的所有子串中，是回文子串的个数；

### 1、Solution-1：Brute Force
1. 考虑s的所有子串：对于每一个，依次判断是否为回文串；

2. Code(https://leetcode.com/submissions/detail/430930500/)
AC
时间复杂度：O(n * n * n)
空间复杂度：O(1)
```C++
class Solution {
private:
    bool isPalindrome(string& s, int i, int j) {
        // 外层调用者保证：j >= i
        while (i < j) {
            if (s[i] != s[j]) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
public:
    int countSubstrings(string s) {
        int n = s.size();
        if (n <= 1) {
            return n;
        }
        
        int ans = 0;
        for (int i = 0; i <= n - 1; i++) {
            for (int j = i; j <= n - 1; j++) {
                if (isPalindrome(s, i, j)) {
                    ans++;
                }
            }
        }
        return ans;
    }
};
```

### 2、Solution-2：Dynamic Programming
1. dp[i][j]表示：子串s[i...j]是否为回文串，不是的话值为0，是的话值为1； 

2. 答案
sum(dp[i][j])；

3. 初始化
dp为n行n列的矩阵；
i > j时，不可能构成子串，dp[i][j] = 0；
i = j时，dp[i][j] = 1；
i < j时，考虑是否可以进行状态转移；

4. 递推式
当i < j时，才需要考虑递推式；
* j = i + 1
只要保证s[i]、s[j]相同，则：子串s[i...j]就是长度为2的回文串；
* j > i + 1
保证s[i]、s[j]相同、且子串s[i + 1...j - 1]是回文串，则：子串s[i...j]就是长度大于2的回文串；

5. 计算顺序
计算第i行，需要第i + 1行的数据；
所以i从下到上遍历；

6. Code(https://leetcode.com/submissions/detail/430932515/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n * n)
```C++
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        if (n <= 1) {
            return n;
        }
        
        int ans = 0;
        
        vector<vector<int>> dp(n, vector<int>(n, 0));
        dp[n - 1][n - 1] = 1;
        ans += 1;
        for (int i = n - 2; i >= 0; i--) {
            dp[i][i] = 1;
            ans += 1;
            for (int j = i + 1; j <= n - 1; j++) {
                if (s[i] == s[j]) {
                    if (j == i + 1) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1] ? 1 : 0;
                    }
                    ans += dp[i][j];
                }
            }
        }
        
        return ans;
    }
};
```

7. 空间优化

8. Code(https://leetcode.com/submissions/detail/430942047/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        if (n <= 1) {
            return n;
        }
        
        int ans = 0;
        
        vector<int> down(n, 0);
        down[n - 1] = 1;
        ans += 1;
        for (int i = n - 2; i >= 0; i--) {
            vector<int> up(n, 0);
            up[i] = 1;
            ans += 1;
            for (int j = i + 1; j <= n - 1; j++) {
                if (s[i] == s[j]) {
                    if (j == i + 1) {
                        up[j] = 1;
                    } else {
                        up[j] = down[j - 1] ? 1 : 0;
                    }
                    ans += up[j];
                }
            }
            up.swap(down);
        }
        
        return ans;
    }
};
```

### 3、Solution-3：Expand Around Center
1. 中心扩展法；

2. expandWithMiddle(s, i, j)含义：
以s[i]、s[j]为中心，能构成几个回文串；

3. Code(https://leetcode.com/submissions/detail/430934388/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
private:
    int expandWithMiddle(string& s, int i, int j) {
        int n = s.size();
        int cnt = 0;
        while (i >= 0 && j <= n - 1 && s[i] == s[j]) {
            i--;
            j++;
            cnt++; // 扩展一次，方案数加一
        }
        return cnt;
    }
public:
    int countSubstrings(string s) {
        int n = s.size();
        if (n <= 1) {
            return n;
        }
        
        int ans = 0;
        
        for (int i = 0; i <= n - 2; i++) {
            ans += expandWithMiddle(s, i, i);
            ans += expandWithMiddle(s, i, i + 1);
        }
        ans += expandWithMiddle(s, n - 1, n - 1);
        
        return ans;
    }
};
```