---
title: Leetcode-1351.Count Negative Numbers in a Sorted Matrix
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
[1351.Count Negative Numbers in a Sorted Matrix](https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix/)

### 1、Description
【输入】
给定一个m * n的矩阵grid；
grid的每行每列都是非升序的；
【输出】
求grid中有多少个负数；
【要求】
无；

### 2、Example
Input: 
[
[4,3,2,-1],
[3,2,1,-1],
[1,1,-1,-2],
[-1,-1,-2,-3]
]
Output: 8
Explanation: There are 8 negatives number in the matrix.

<!-- more -->

### 3、Corner Case
1. m == 0 || n == 0的时候，答案为0；
2. 1 <= m, n <= 100
3. -100 <= grid[i][j] <= 100

## 二、题解

### 1、Solution-1：Brute Force
1. 没什么好说的，暴力就完事了。

2. Code(https://leetcode.com/submissions/detail/327587530/)
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int cnt = 0;
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (grid[i][j] < 0) {
                    cnt++;
                }
            }
        }
        return cnt;
    }
};
```

3. 优化方法1：在暴力的基础上，利用grid每行每列都是有序（非升序）的特性。
从右下角开始找（负数的个数）：
如果某个grid[i][n - 1] >= 0。那么第0行-第i行都不可能存在负数了。i跳出外循环；
如果某个grid[i][j] >= 0。那么该行的第0列-第j列都不可能存在非负数了。j跳出内循环；

4. Code(https://leetcode.com/submissions/detail/327586460/)
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        // Solution-2-1: 找出负数的个数cnt；
        int cnt = 0;
        for (int i = m - 1; i >= 0 && grid[i][n - 1] <= -1; i--) {
            for (int j = n - 1; j >= 0 && grid[i][j] <= -1 ; j--) {
                cnt++;
            }
        }
        return cnt;
    }
};
```

5. 优化方法2：在暴力的基础上，利用grid每行每列都是有序（非升序）的特性。
从左上角开始找（非负数的个数）：
如果某个grid[i][0] < 0。那么第i行-第m - 1行都不可能存在非负数了。i跳出外循环；
如果某个grid[i][j] < 0。那么该行的第j列-第n - 1列都不可能存在非负数了。j跳出内循环；

6. Code(https://leetcode.com/submissions/detail/327585411/)
时间复杂度：O(m * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        // Solution-2-2: 找出非负数的个数cnt，答案即为：m * n - cnt；
        int cnt = 0;
        for (int i = 0; i <= m - 1 && grid[i][0] >= 0; i++) {
            for (int j = 0; j <= n - 1 && grid[i][j] >= 0 ; j++) {
                cnt++;
            }
        }
        return m * n - cnt;
    }
};
```

### 2、Solution-2：Binary Search
1. 找出每行负数的个数，然后累加起来即可。
由于每行都是有序的（非升序），所以用二分查找可以将时间复杂度由O(n)降低为O(log n)。

2. upper_bound(first, last, value)：返回首个大于value的元素的迭代器，若找不到这种元素则为last。

3. 缺点：只利用了行有序的特点；没利用列也是有序的特点；

4. Code(https://leetcode.com/submissions/detail/327662171/)
时间复杂度：O(m * log n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int cnt = 0;
        for (int i = 0; i <= m - 1; i++) {
            cnt += (upper_bound(grid[i].rbegin(), grid[i].rend(), -1) - grid[i].rbegin());
        }
        return cnt;
    }
};
```

### 3、Solution-3：Two Pointers
1. 双指针法：(i, j)开始指向grid左下角的元素：grid[m - 1][0]。

2. 从左下角开始遍历：每次要么往上走一步，要么往右走一步。最多走m + n步。

3. 过程：
// 如果当前位置的元素是非负数，往右走一步(c++)，直到找到一个负数为止。若c出界，跳出while循环。
// 如果当前位置的元素(i, j)是负数，则(i, j - 1)一定是非负数，或者在grid外面。原因如下：
// 1. 当前位置是出发点。那么(i, j - 1)在grid外面，不予考虑。累加本行的负数个数即可(n - c)，同时r--；
// 2. 当前位置不是出发点，是由出发点经过若干步骤跳过来的。
// 2-1. 往右跳，到达当前位置的。那么(i, j - 1)一定是非负数。
// 2-2. 往上跳，到达当前位置的。那么(i + 1, j)是第i + 1行最大的负数。可得(i + 1, j - 1)是非负数。可得(i, j - 1)是非负数。
// 所以当前位置如果是负数的话，不考虑其左边的所有元素，直接累加本行的负数个数即可。

4. Code(https://leetcode.com/submissions/detail/327611749/)
时间复杂度：O(m + n)
空间复杂度：O(1)
```C++
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int cnt = 0;
        int r = m - 1;
        int c = 0;
        while (r >= 0 && c <= n - 1) {
            if (grid[r][c] <= -1) {
                cnt += n - c;
                r--;
            } else {
                c++;
            }
        }
        return cnt;
    }
};
```

















