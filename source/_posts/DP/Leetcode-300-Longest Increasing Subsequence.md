---
title: Leetcode-300.Longest Increasing Subsequence
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Dynamic Programming
- Binary Search
---

## 一、题意

### 0、题目链接
[300.Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

### 1、Description
【输入】
给定一个长度为n的数组nums；
【输出】
求出nums的所有上升子序列中，最长的那个，输出这个长度；
上升子序列：后一个元素严格大于前一个元素；
【要求】
无；

### 2、Example
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 2500
2. n == 1时，输出nums[0]；

## 二、题解

### 0、思考
1. 【子序列】不要求连续，所以相较于【子数组】来说，数量更多。
对于一个长度为n的数组来说：
【子数组】（非空）的个数为：n * (n + 1) / 2；复杂度：O(n * n)
【子序列】（非空）的个数为：(2 ^ n) - 1；复杂度：O(2 ^ n)

### 1、Solution-1：Dynamic Programming
1. dp[i]表示：以nums[i]结尾的所有上升子序列中，最大的长度；

2. 答案
max(dp[0]-dp[n - 1])；

3. 初始化
dp[0] = 1；

4. 递推式
dp[i] = max(dp[i], dp[j] + 1);
解释：对于nums[i]，要想构成上升子序列，需要考虑其上一个元素：nums[0...i - 1]；

5. Code(https://leetcode.com/submissions/detail/430028067/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 1;
        }
        
        vector<int> dp(n);
        dp[0] = 1;
        int ans = 1;
        for (int i = 1; i <= n - 1; i++) {
            dp[i] = 1;
            for (int j = 0; j <= i - 1; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

### 2、Solution-2：Dynamic Programming + Search
0. 经典问题：LIS

1. tail[i]表示：长度为i + 1的满足条件的若干个序列中，序列末尾元素的最小值

2. 依次考虑nums[1...n - 1]：
* 当前元素nums[i]，可以加入到当前的最长上升子序列末尾；
* 当前元素nums[i]，不能加入到当前的最长上升子序列末尾；但可以尝试优化tail[0...maxLen - 1]；

3. 现象：若同时有2个子序列(1, 5)和(1, 10)，然后取一个随机数x接在后面，能构成长度为3的递增子序列吗？
肯定是可以的，例如：
x > 10 :都ok
x <= 5 :都不行
x == 6/7/8/9/10 :(1, 5)来说ok，(1, 10)来说不行
即：对于(1, 5)和(1, 10)来说，能扩展成长度为3的递增子序列的“成功率”是不相等的，且(1, 5)的“扩展性”更好。

4. 基于以上现象，得出优化方法：
找到tail中第一个大于等于nums[i]的元素，将其修改为nums[i]

5. 同时，更加巧妙的是：tail数组是有序的；

6. Code(https://leetcode.com/submissions/detail/430055392/)
AC
时间复杂度：O(n * n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 1;
        }
        
        // tail[i]：长度为i + 1的满足条件的若干个序列中，序列末尾元素的最小值
        vector<int> tail(n);
        tail[0] = nums[0];
        int maxLen = 1; 
        for (int i = 1; i <= n - 1; i++) {
            if (nums[i] > tail[maxLen - 1]) {
                // nums[i]可以加入到当前的最长上升子序列末尾
                tail[maxLen] = nums[i];
                maxLen++;
            } else {
                // nums[i]不能加入到当前的最长上升子序列末尾
                // 但可以尝试优化tail[0...maxLen - 1]
                // 优化方法：找到tail中第一个大于等于nums[i]的元素，将其修改为nums[i]
                int idx;
                for (idx = 0; idx <= maxLen - 1; idx++) {
                    if (tail[idx] >= nums[i]) {
                        break;
                    }
                }
                tail[idx] = nums[i];
            }
        }
        return maxLen;
    }
};
```

### 3、Solution-3：Dynamic Programming + Binary Search
1. 由于tail数组是有序的，所以可以进行二分搜索；

2. 二分搜索模板2：找到第一个满足条件的元素；

3. Code(https://leetcode.com/submissions/detail/430058144/)
AC
时间复杂度：O(n * log n)
空间复杂度：O(n)
```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 1;
        }
        
        // tail[i]：长度为i + 1的满足条件的若干个序列中，序列末尾元素的最小值
        vector<int> tail(n);
        tail[0] = nums[0];
        int maxLen = 1; 
        for (int i = 1; i <= n - 1; i++) {
            if (nums[i] > tail[maxLen - 1]) {
                // nums[i]可以加入到当前的最长上升子序列末尾
                tail[maxLen] = nums[i];
                maxLen++;
            } else {
                // nums[i]无法加入到当前的最长上升子序列末尾
                // 但可以尝试优化tail[0...maxLen - 1]
                // 优化方法：找到tail中第一个大于等于nums[i]的元素，将其修改为nums[i]
                int left = 0;
                int right = maxLen;
                while (left < right) {
                    int mid = left + (right - left) / 2;
                    if (tail[mid] >= nums[i]) {
                        right = mid;
                    } else {
                        left = mid + 1;
                    }
                }
                tail[left] = nums[i];
            }
        }
        return maxLen;
    }
};
```