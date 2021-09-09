---
title: Leetcode-005.Longest Palindromic Substring
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
- Two Pointers
---

## 一、题意

### 0、题目链接
[005.Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

### 1、Description
【输入】
给定一个长度为n的字符串s；
【输出】
求出s最长的那个回文子串；
若存在多个，任意输出一个即可；
【要求】
无；

### 2、Example
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 1000
2. n == 1时，输出s；

## 二、题解

### 1、Solution-1：Brute Force
1. 考虑s的所有子串：对于每一个，依次判断是否为回文串；

2. Code(https://leetcode.com/submissions/detail/430581510/)
AC
时间复杂度：O(n * n * n)
空间复杂度：O(1)
```C++
class Solution {
private:
    bool isPalindrome(string& s, int l, int r) {
        while (l < r) {
            if (s[l] != s[r]) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n == 1) {
            return s; 
        }
        
        int maxLen = 1;
        int idx = 0;
        for (int i = 0; i <= n - 2; i++) {
            for (int j = i + 1; j <= n - 1; j++) {
                if (j - i + 1 > maxLen && isPalindrome(s, i, j)) {
                    maxLen = j - i + 1;
                    idx = i;
                }
            }
        }
        
        return s.substr(idx, maxLen);
    }
};
```

### 2、Solution-2：Dynamic Programming
1. dp[i][j]表示：子串s[i...j]是否为回文串，不是的话值为0，是的话值为其长度；

2. 答案
max(dp[i][j])；

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
保证s[i]、s[j]相同、且子串s[i + 1...j - 1]是回文串，则：子串s[i...j]就是长度dp[i + 1][j - 1] + 2的回文串；

5. 计算顺序
计算第i行，需要第i + 1行的数据；
所以i从下到上遍历；

6. Code(https://leetcode.com/submissions/detail/430585862/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n * n)
```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n == 1) {
            return s; 
        }
        
        vector<vector<int>> dp(n, vector<int>(n, 0));
        dp[n - 1][n - 1] = 1;
        int maxLen = 1;
        int idx = 0;
        
        for (int i = n - 2; i >= 0; i--) {
            dp[i][i] = 1;
            for (int j = i + 1; j <= n - 1; j++) {
                if (s[i] == s[j]) {
                    if (j == i + 1) {
                        dp[i][j] = 2;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1] ? dp[i + 1][j - 1] + 2 : 0;
                    }
                    if (dp[i][j] > maxLen) {
                        maxLen = dp[i][j];
                        idx = i;
                    }
                }           
            }
        }
        
        return s.substr(idx, maxLen);
    }
};
```

7. 空间优化

8. Code(https://leetcode.com/submissions/detail/430591729/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n == 1) {
            return s; 
        }
        
        vector<int> down(n, 0);
        down[n - 1] = 1;
        int maxLen = 1;
        int idx = 0;
        
        for (int i = n - 2; i >= 0; i--) {
            vector<int> up(n);
            up[i] = 1;
            for (int j = i + 1; j <= n - 1; j++) {
                if (s[i] == s[j]) {
                    if (j == i + 1) {
                        up[j] = 2;
                    } else {
                        up[j] = down[j - 1] ? down[j - 1] + 2 : 0;
                    }
                    if (up[j] > maxLen) {
                        maxLen = up[j];
                        idx = i;
                    }
                }           
            }
            up.swap(down);
        }
        
        return s.substr(idx, maxLen);
    }
};
```

### 3、Solution-3：Expand Around Center
1. Solution-2中，我们枚举子串的左右两个端点；

2. Solution-3中，我们枚举子串的中心点；

3. 对于s[0, 1, 2, ..., n - 1]来说：
* 中心点是1个：例如回文串"121"
以s[i]为中心点，进行扩展，找到回文串的最大长度；
i : [0, n - 1]
由于以s[n - 1]为中心点的回文串长度一定为1，所以可以不用考虑；
i : [0, n - 2]
* 中心点是2个：例如回文串"3223"
以s[i]、s[i + 1]为中心点，进行扩展，找到回文串的最大长度；
i : [0, n - 2]

4. Code(https://leetcode.com/submissions/detail/430599971/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
private:
    int expandWithMiddle(string& s, int l, int r) {
        int n = s.size();
        while (l >= 0 && r <= n - 1 && s[l] == s[r]) {
            l--;
            r++;
        }
        return r - l - 1;
    }
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n == 1) {
            return s; 
        }
        
        int maxLen = 1;
        int idx = 0;
        
        for (int i = 0; i <= n - 2; i++) {
            int oddLen = expandWithMiddle(s, i, i);
            int evenLen = expandWithMiddle(s, i, i + 1);
            if (max(oddLen, evenLen) > maxLen) {
                if (oddLen > evenLen) {
                    maxLen = oddLen;
                    idx = i - oddLen / 2;
                } else {
                    maxLen = evenLen;
                    idx = i - (evenLen - 2) / 2;
                }
            }
        }
        
        return s.substr(idx, maxLen);
    }
};
```

### 4、Solution-4：Manacher's Algorithm
TO DO
时间复杂度：O(n)
空间复杂度：O(n)