---
title: Leetcode-739.Daily Temperatures
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Stack
- Monotonous Stack
---

## 一、题意

### 0、题目链接
[739.Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

### 1、Description
【输入】
给定一个大小为n的整数数组T；
T[i]代表：第i天的气温；
【输出】
求出每一天，分别需要等待几天，才能见到一个更暖和的天气；以一维数组的形式返回；
如果等不到，则记为0；
【要求】
无；

### 2、Example
Input: T = [73, 74, 75, 71, 69, 72, 76, 73]；
Output: ans = [1, 1, 4, 2, 1, 1, 0, 0]；

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 30000
2. 30 <= T[i] <= 100
3. n == 1时，返回{0}；
4. ans[n - 1] == 0；

## 二、题解

### 0、思考
1. 本质上就是求：每个元素右边第一个【比我大】的元素的索引；

### 1、Solution-1：Brute Force
1. i从0开始，到n - 2结束。

2. j从i + 1开始，到n - 1结束。

3. Code(https://leetcode.com/submissions/detail/348021384/)
TLE
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int n = T.size();
        if (n == 1) {
            return {0};
        }
        
        vector<int> ans(n, 0);
        for (int i = 0; i <= n - 2; i++) {
            for (int j = i + 1; j <= n - 1; j++) {
                if (T[j] > T[i]) {
                    ans[i] = j - i;
                    break;
                }
            }
        }
        
        return ans;
    }
};
```

### 2、Solution-2：Monotonous Stack
1. 【单调栈】介绍：
一方面，很难思考出来，很巧妙；
另一方面，应用范围较小，所以很好解决问题；所谓“会者不难”；

2. 用于解决这类问题：
查找每个数左（右）侧第一个【比我小（大）】的数；

3. 在本题目中：查找每个数右侧第一个【比我大】的数的index；
对应的单调栈为：单调递减，即栈顶元素最小。

4. 举个例子：T = [73, 74, 75, 71, 69, 72, 76, 73]，n = 8；
i = 0：[(0, 73) ；
i = 1：[(1, 74) ；ans[0] = 1 - 0 = 1；
i = 2：[(2, 75) ；ans[1] = 2 - 1 = 1；
i = 3：[(2, 75)(3, 71)；
i = 4：[(2, 75)(3, 71)(4, 69)；
i = 5：[(2, 75)(5, 72)；ans[4] = 5 - 3 = 1；ans[3] = 5 - 3 = 2；
i = 6：[(6, 76)；ans[5] = 6 - 5 = 1；ans[2] = 6 - 2 = 4；
i = 7：[(6, 76)(7, 73)；
ans = [1, 1, 4, 2, 1, 1, 0, 0]

5. 观察：
如果某个元素在栈中，代表：其目标值还没出现；
如果某个元素出栈了，代表：其目标值已出现，我对应的答案计算出来了；

6. Code(https://leetcode.com/submissions/detail/348031262/)
AC
时间复杂度：O(n) // 虽然有内外两层循环，但每个元素最多只入栈一次、出栈一次。
空间复杂度：O(n) // 最坏情况下，所有元素都入栈。
```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int n = T.size();
        if (n == 1) {
            return {0};
        }
        
        vector<int> ans(n, 0);
        stack<int> s;
        for (int i = 0; i <= n - 1; i++) {
            while (!s.empty() && T[i] > T[s.top()]) {
                int idx = s.top();
                ans[idx] = i - idx;
                s.pop();
            }
            s.push(i);
        }
        
        return ans;
    }
};
```