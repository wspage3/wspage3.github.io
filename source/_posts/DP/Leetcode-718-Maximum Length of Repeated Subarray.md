---
title: Leetcode-718.Maximum Length of Repeated Subarray
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
---

## 一、题意

### 0、题目链接
[718.Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

### 1、Description
【输入】
给定两个数组A、B，长度分别为na、nb；
【输出】
求A和B的最长公共子数组的长度；
LCs（Longest Common sub-array）；
【要求】
无；

### 2、Example
Input:
A: [1,2,3,2,1]
B: [3,2,1,4,7]
Output: 3
Explanation: 
The repeated subarray with maximum length is [3, 2, 1].

<!-- more -->

### 3、Corner Case
1. 1 <= na、nb <= 1000

## 二、题解

### 0、思考
1. 经典问题LCS（最长公共子序列）：Leetcode-1143-Longest Common Subsequence

2. 经典问题LCs（最长公共子数组）：本题

### 1、Solution-1：Dynamic Programming
1. dp[i][j]表示：以A[i - 1]结尾、以B[j - 1]结尾的最长公共子数组的长度；

2. 答案：
max(dp[i][j])；

3. 初始化
dp为na + 1行、nb + 1列的矩阵；
第一行、第一列初始化为0；
i从1开始，到na结束、j从1开始，到nb结束，计算；

4. 递推式
* A[i - 1] == B[j - 1]
dp[i][j] = dp[i - 1][j - 1] + 1
* A[i - 1] == B[j - 1]
dp[i][j] = 0

5. Code(https://leetcode.com/submissions/detail/431570927/)
AC
时间复杂度：O(na * nb)
空间复杂度：O(na * nb)
```C++
class Solution {
public:
    int findLength(vector<int>& A, vector<int>& B) {
        int na = A.size();
        int nb = B.size();
        int maxLen = 0;
        vector<vector<int>> dp(na + 1, vector<int>(nb + 1, 0));
        for (int i = 1; i <= na; i++) {
            for (int j = 1; j <= nb; j++) {
                if (A[i - 1] == B[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                    maxLen = max(maxLen, dp[i][j]);
                }
            }
        }
        return maxLen;
    }
};
```

6. 空间优化
* 二维空间降为一维空间；
* 选择min(na, nb)作为一维空间长度；

7. Code(https://leetcode.com/submissions/detail/431573212/)
AC
时间复杂度：O(na * nb)
空间复杂度：O(min(na, nb))
```C++
class Solution {
public:
    int findLength(vector<int>& A, vector<int>& B) {
        int na = A.size();
        int nb = B.size();
        if (nb > na) {
            return findLength(B, A);
        }
        
        int maxLen = 0;
        vector<int> dp(nb + 1, 0);
        for (int i = 1; i <= na; i++) {
            for (int j = nb; j >= 1; j--) { // 防止覆盖有用数据，从后往前遍历
                if (A[i - 1] == B[j - 1]) {
                    dp[j] = dp[j - 1] + 1;
                    maxLen = max(maxLen, dp[j]);
                } else {
                    dp[j] = 0; // 清0，为了下一行使用
                }
            }
        }
        return maxLen;
    }
};
```