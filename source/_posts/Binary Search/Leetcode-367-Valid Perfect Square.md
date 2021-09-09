---
title: Leetcode-367.Valid Perfect Square
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Math
- Binary Search
---

## 一、题意

### 0、题目链接
[367.Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/)

### 1、Description
【输入】
给定一个正整数num；
【输出】
判断num是否是一个完全平方数，返回true或false；
【要求】
不允许使用内置的库函数；

### 2、Example
Input: 16
Output: true

Input: 14
Output: false

<!-- more -->

### 3、Corner Case
1. num > 0；

## 二、题解

### 0、思考
1. 完全平方数：如果一个正整数 a 是某一个整数 b 的平方，那么这个正整数 a 叫做完全平方数。零也可称为完全平方数。
即：[0, 1, 4, 9, 16, 25, ...]

### 1、Solution-1：Binary Search
1. 在069.Sqrt(x)中，给定一个非负数n，我们可以在O(log n)的时间内，计算n的平方根（仅保留整数部分）。

2. 现在num是正数，先求出其平方根ans，再看ans * ans是否等于num。如果相等，说明num是一个完全平方数。

3. num在范围[1, 2147483647]内。当num == 2147483647时，ans = 46340，ans * ans = 2147395600不会造成overflow。‬

4. Code(https://leetcode.com/submissions/detail/331822667/)
AC
时间复杂度：O(log(num))
空间复杂度：O(1)
```C++
class Solution {
private:
    int mySqrt(int x) {
        if (x == 0) {
            return 0;
        }
        if (x == 1) {
            return 1;
        }
        // 考虑x >= 2的情况：
        int left = 1;
        int right = x;
        // 注意：按照二分模板-3，我们应该在[left, right + 1)上进行搜索。其中[left, right]是target所有可能的取值空间。
        // 由于x >= 2，所以：left最小为1；right最大为x - 1。
        // 所以我们在：[1, x - 1 + 1)中搜索。避免了x为INT_MAX导致overflow的情况。
        while (left < right) {
            int mid = left + (right - left) / 2;
            // mid * mid <= x 会overflow，转换为：
            // mid <= x / mid
            if (mid <= x / mid) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // 此时left == right。取值范围为：[1, x]。
        // 当left == 1的时候，说明搜索区间内没有满足条件的数字。
        // 而1一定在搜索空间内，对于x（大于等于2），一定满足1 * 1 <= x；
        // 所以此时left/right的取值范围为：[2, x]。
        return left - 1;
    }
public:
    bool isPerfectSquare(int num) {
        int ans = mySqrt(num);
        return ans * ans == num;
    }
};
```

### 2、Solution-2：Brute Force
1. 对于正数num，i枚举[1, num]，看能否找到一个i，使得i * i == num。如果找到了，说明num是完全平方数。

2. 当i * n > num的时候，说明num不是完全平方数，返回false。

3. Code(https://leetcode.com/submissions/detail/331829686/)
AC
时间复杂度：O(sqrt(num))
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isPerfectSquare(int num) {
        for (long long i = 1; i <= num; i++) {
            if (i * i == num) {
                return true;
            } else if (i * i > num) {
                return false;
            }
        }
        return false; // cannot execute
    }
};
```

### 3、Solution-3：Math
1. 观察：
1 = 1
4 = 1 + 3
9 = 1 + 3 + 5
16 = 1 + 3 + 5 + 7
25 = 1 + 3 + 5 + 7 + 9
36 = 1 + 3 + 5 + 7 + 9 + 11
....
so 1 + 3 + ... + (2n-1) = (2n-1 + 1)n/2 = n * n

2. 所以若num是完全平方数，n是其平方根，则num可以写成长度为n的等差数列。
公差为：2。
首项为：1。
尾项为：2 * n - 1。
这个等差数列的和刚好为：num。

3. 利用上述特性，进行判断。

4. Code(https://leetcode.com/submissions/detail/331833814/)
AC
时间复杂度：O(sqrt(num))
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isPerfectSquare(int num) {
        int i = 1;
        while (num > 0) {
            num = num - i;
            i = i + 2;
        }
        return num == 0;
    }
};
```

### 4、Solution-4：Binary Search
1. 类似于Solution-2，可以在区间[1, num]搜索，看满足条件的那个数是否存在。二分模板-1。

2. 条件：找到一个i，使得i * i == num。

3. Code(https://leetcode.com/submissions/detail/331836348/)
AC
时间复杂度：O(log(num))
空间复杂度：O(1)
```C++
class Solution {
public:
    bool isPerfectSquare(int num) {
        int left = 1;
        int right = num;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (num / mid == mid && num % mid == 0) {
                return true;
            } else if (mid > num / mid) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return false;
    }
};
```