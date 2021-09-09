---
title: Leetcode-402.Remove K Digits
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Stack
- Greedy
---

## 一、题意

### 0、题目链接
[402.Remove K Digits](https://leetcode.com/problems/remove-k-digits/)

### 1、Description
【输入】
给定一个长度为n的字符串num，代表一个非负整数；
给定一个整数k；
【输出】
在字符串num中，删除k个字符，使得形成的数字最小；
输出这个数字；
【要求】
无；

### 2、Example
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.

<!-- more -->

### 3、Corner Case
1. k <= n；
2. 当n == 0时，不能表示num为一个整数，无意义；
3. 当k == 0时，直接返回num；
4. 当k == n时，直接返回"0"；
5. num中没有前置0；

## 二、题解

### 0、思考
1. 观察：对于124859来说：如果仅删除一位，那么删除哪一位，才会使得新数字最小？
删除8后，变成：12459最小。

2. 不难得出结论：每次删除num的“极大值”即可。即这个数字比它两边的数字都要大。

### 1、Solution-1：Brute Force
1. 直接遍历k次，cnt从1开始，到k结束；

2. i从0开始，到n - 1 - cnt结束。每次判断num[i]、num[i + 1]的大小。
若num[i] < num[i + 1]，则i++；
若num[i] == num[i + 1]，则i++；
若num[i] > num[i + 1]，则break；
最终i停留的位置即：极大值的索引；从num中把该字符删除；

3. 最后，去除前置的0。

4. Code(https://leetcode.com/submissions/detail/346118298/)
AC
时间复杂度：O(k * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        int n = num.size();
        if (k == 0) {
            return num;
        }
        if (k == n) {
            return "0";   
        }
        
        for (int cnt = 1; cnt <= k; cnt++) {
            int i = 0;
            while (i <= n - 1 - cnt && num[i] <= num[i + 1]) {
                i++;
            }
            num.erase(i, 1);
        }
        
        int i = 0;
        while (i <= n - k - 1 && num[i] == '0') {
            i++;
        }
        num.erase(0, i);
        
        return num == "" ? "0" : num;
    }
};
```

### 2、Solution-2：Using Stack
1. 举例：对于num = "124859"，k = 3来说，过程如下：

2. 初始化：ans = []，k = 3；
i = 0：ans = [1]；
i = 1：ans = [12]；
i = 2：ans = [124]；
i = 3：ans = [1248]；
i = 4：ans = [1245]，k = 2；
i = 5：ans = [12459]；

3. 此时，ans中都是从小到大排序好了。
但可能删除的字符不足k个，需要继续删除ans的后几个字符。ans = [124]。
也可能删除的字符达到k个，则不需要删除了；

4. 最后，去除前置0；

5. Code(https://leetcode.com/submissions/detail/346130953/)
AC
时间复杂度：O(n) // 内循环最多执行k次，而k <= n；
空间复杂度：O(n)
```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        int n = num.size();
        if (k == 0) {
            return num;
        }
        if (k == n) {
            return "0";   
        }
        
        int remainCnt = n - k;
        string ans = "";
        for (int i = 0; i <= n - 1; i++) {
            while (ans.size() >= 1 && ans.back() > num[i] && k > 0) {
                ans.pop_back();
                k--;
            }
            ans.push_back(num[i]);
        }
        
        ans.erase(remainCnt, ans.size() - remainCnt);
        
        int i = 0;
        while (i <= n - k - 1 && ans[i] == '0') {
            i++;
        }
        ans.erase(0, i);
        
        return ans == "" ? "0" : ans;
    }
};
```

