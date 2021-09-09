---
title: Leetcode-074.Search a 2D Matrix
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
[074.Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

### 1、Description
【输入】
给定一个m * n的矩阵matrix；
matrix的每行每列都是升序的；
同时matrix中每行的第一个数字都比上一行最后一个数字大；
给定一个整数target；
【输出】
判断target是否出现在matrix中。返回true或者false。
【要求】
无；

### 2、Example
Input:
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false

<!-- more -->

### 3、Corner Case
1. m == 0 || n == 0的时候，答案为false；
2. 考虑m、n均为正数的情况；

## 二、题解

### 0、思考
1. 类似于240.Search a 2D Matrix II。
2. 240中matrix每行每列都是升序的。本题中matrix不仅每行每列都是升序的，而且每行的第一个数字都比上一行最后一个数字大。

### 1、Solution-1：Two Pointers
1. 双指针法：(i, j)开始指向matrix左下角的元素：matrix[m - 1][0]。

2. 从左下角开始遍历：每次要么往上走一步，要么往右走一步。最多走m + n步。

3. Code(https://leetcode.com/submissions/detail/330434862/)
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
        int m = matrix.size();
        int n = matrix[0].size();
        int r = m - 1;
        int c = 0;
        while (r >= 0 && c <= n - 1) {
            if (matrix[r][c] == target) {
                return true;
            } else if (matrix[r][c] < target) {
                c++;
            } else {
                r--;
            }
        }
        return false;
    }
};
```

### 2、Solution-2：Binary Search
1. 由于matrix的特殊性质，可以把其看作是一个一维数组，而且是有序的。

2. 在一维数组[0, m * n - 1]上作二分查找；

3. 在(i, j)位置时，可以得到一维的idx = i * n + j。

4. 当已知一维idx时，可以推算出二维的坐标：(idx / n, idx % n)。

5. 二分搜索边界值：
当搜索空间大小为3时：[left, mid, right]。首先判断mid：1.找到；2.left = mid + 1；3.right = mid - 1；后两种情况会继续搜索（搜索空间大小为1）。
当搜索空间大小为2时：[left, right]，mid = left。首先判断mid：1.找到；2.left = mid + 1；3.right = mid - 1；第二种情况会继续搜索；第三种情况不会继续进入循环，没找到target。
当搜索空间大小为1时：left = right = mid。直接判断mid即可：1.找到；2.left = mid + 1；3.right = mid - 1；后两种情况不会继续进入循环，没找到target。

6. Code(https://leetcode.com/submissions/detail/330445434/)
AC
时间复杂度：O(log(m * n))
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
        int left = 0;
        int right = m * n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int i = mid / n;
            int j = mid % n;
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
};
```

### 3、Solution-3：Binary Search
1. Solution-2中的方法存在一定的缺点：m * n可能会导致overflow。

2. i在[0, m - 1]进行二分查找：找到最小的i，使得matrix[i][n - 1] >= target。

3. j在[0, n - 1]进行二分查找，判断target是否存在。

4. Code(https://leetcode.com/submissions/detail/330743835/)
AC
时间复杂度：O(log(m) + log(n))
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
        
        // 1. i在[0, m)进行二分查找：找到那个最小的i，满足条件：matrix[i][n - 1] >= target。（二分模板2）
        int l = 0;
        int r = m;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (target == matrix[mid][n - 1]) {
                r = mid;
            } else if (target > matrix[mid][n - 1]) {
                l = mid + 1;
            } else if (target < matrix[mid][n - 1]) {
                r = mid;
            }
        }
        // 此时一定有：l == r；其取值范围是：[0, m]。
        if (l == m) {
            // 如果l == m，说明target比所有matrix[i][n - 1]都大。
            return false;
        }
        
        // 2. 记录下target可能存在的行的索引。
        int row = l;
        
        // 3. j在[0, n - 1]进行二分查找，判断target是否存在。（二分模板1）
        l = 0;
        r = n - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (target == matrix[row][mid]) {
                return true;
            } else if (target < matrix[row][mid]) {
                r = mid - 1;
            } else if (target > matrix[row][mid]) {
                l = mid + 1;
            }
        }
        return false;
    }
};
```















