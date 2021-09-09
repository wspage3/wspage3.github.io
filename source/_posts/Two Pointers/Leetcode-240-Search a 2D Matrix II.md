---
title: Leetcode-240.Search a 2D Matrix II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
- Two Pointers
---

## 一、题意

### 0、题目链接
[240.Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

### 1、Description
【输入】
给定一个m * n的矩阵matrix；matrix的每行每列都是升序的；
给定一个整数target；
【输出】
判断target是否出现在matrix中。返回true或者false。
【要求】
无；

### 2、Example
Input: 
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
Given target = 5, return true.
Given target = 20, return false.

<!-- more -->

### 3、Corner Case
1. m == 0 || n == 0的时候，答案为false；
2. 考虑m、n均为正数的情况；

## 二、题解

### 0、思考
1. 类似于1351.Count Negative Numbers in a Sorted Matrix；
2. 都是在一个二维matrix中搜索，且matrix每行每列都是有序的。
3. 本题搜索一个元素是否存在。1351搜索小于某个值的元素的个数。

### 1、Solution-1：Brute Force
1. 没什么好说的，暴力就完事了。

2. Code(https://leetcode.com/submissions/detail/327577449/)
TLE
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return false;
        }
        int m = matrix.size();
        int n = matrix[0].size();
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (matrix[i][j] == target) {
                    return true; 
                }
            }
        }
        return false;
    }
};
```

3. 优化方法1：在暴力的基础上，利用matrix每行每列都是有序（升序）的特性。
从右下角开始找：
如果某个matrix[i][n - 1] < target。那么第0行-第i行都不可能存在target了。i跳出外循环；
如果某个matrix[i][j] < target。那么该行的第0列-第j列都不可能存在target了。j跳出内循环；

4. Code(https://leetcode.com/submissions/detail/327664649/)
勉强AC
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return false;
        }
        // Solution-2-1: 从右下角开始找。
        int m = matrix.size();
        int n = matrix[0].size();
        for (int i = m - 1; i >= 0 && matrix[i][n - 1] >= target; i--) {
            for (int j = n - 1; j >= 0 && matrix[i][j] >= target; j--) {
                if (matrix[i][j] == target) {
                    return true; 
                }
            }
        }
        return false;
    }
};
```

5. 优化方法2：在暴力的基础上，利用matrix每行每列都是有序（升序）的特性。
从左上角开始找：
如果某个matrix[i][0] > target。那么第i行-第m - 1行都不可能存在target了。i跳出外循环；
如果某个matrix[i][j] > target。那么该行的第j列-第n - 1列都不可能存在target了。j跳出内循环；

6. Code(https://leetcode.com/submissions/detail/327665776/)
TLE
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return false;
        }
        // Solution-2-2: 从左上角开始找。        
        int m = matrix.size();
        int n = matrix[0].size();
        for (int i = 0; i <= m - 1 && matrix[i][0] <= target; i++) {
            for (int j = 0; j <= n - 1 && matrix[i][j] <= target; j++) {
                if (matrix[i][j] == target) {
                    return true; 
                }
            }
        }
        return false;
    }
};
```

### 2、Solution-2：Binary Search
1. 判断每行是否存在target。
由于每行都是有序的（升序），所以用二分查找可以将时间复杂度由O(n)降低为O(log n)。

2. lower_bound(first, last, value)：返回首个大于等于value的元素的迭代器；若找不到这种元素则为last；

3. 缺点：只利用了行有序的特点；没利用列也是有序的特点；

4. Code(https://leetcode.com/submissions/detail/327674767/)
AC
时间复杂度：O(m * log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return false;
        }
        // Solution-2: Binary Search      
        int m = matrix.size();
        int n = matrix[0].size();
        for (int i = 0; i <= m - 1; i++) {
            // 找到第一个 >= target 的index。
            int step = lower_bound(matrix[i].begin(), matrix[i].end(), target) - matrix[i].begin();
            if (step == n || matrix[i][step] != target) {
                continue;
            }
            return true;
        }
        return false;
    }
};
```

### 3、Solution-3：Two Pointers
1. 双指针法：(i, j)开始指向matrix左下角的元素：matrix[m - 1][0]。

2. 从左下角开始遍历：每次要么往上走一步，要么往右走一步。最多走m + n步。

3. Code(https://leetcode.com/submissions/detail/327671824/)
AC
时间复杂度：O(m + n)
空间复杂度：O(1)
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return false;
        }
        // Solution-3: Two Pointers        
        int m = matrix.size();
        int n = matrix[0].size();
        int r = m - 1;
        int c = 0;
        while (r >= 0 && c <= n - 1) {
            if (matrix[r][c] == target) {
                return true;
            } else if (matrix[r][c] > target) {
                r--;
            } else {
                c++;
            }
        }
        return false;
    }
};
```

















