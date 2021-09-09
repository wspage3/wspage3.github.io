---
title: Leetcode-048.Rotate Image
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
---

## 一、题意

### 0、题目链接
[048.Rotate Image](https://leetcode.com/problems/rotate-image/)

### 1、Description
【输入】
给定一个n * n的矩阵matrix；
【输出】
对matrix进行顺时针旋转90°；
【要求】
in-place：不得申请额外空间，O(1)的空间复杂度；

### 2、Example
Input: matrix = 
[
    [1,2,3],
    [4,5,6],
    [7,8,9]
]
Output: 
[
    [7,4,1],
    [8,5,2],
    [9,6,3]
]

Input: matrix = 
[
    [5,1,9,11],
    [2,4,8,10],
    [13,3,6,7],
    [15,14,12,16]
]
Output: 
[
    [15,13,2,5],
    [14,3,4,1],
    [12,6,8,9],
    [16,7,10,11]
]

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 20
2. n == 1时，无需操作；

## 二、题解

### 1、Solution-1：Straightforward
1. 类似Leetcode-054.Spiral Matrix中：
我们依次考虑每一层，因为不同层之间互不影响；

2. 层数：n / 2，例如：
n = 3：1层
n = 4：2层
n = 5：2层

3. 不难发现：对于点A(i, j)，顺时针旋转90°后，新坐标为：(j, n - 1 - i)，记为点B；
A: (i, j)  -->  B: (j, n - 1 - i) 
B: (j, n - 1 - i)  -->  C: (n - 1 - i, n - 1 - j)
C: (n - 1 - i, n - 1 - j)  -->  D: (n - 1 - j, i)
D: (n - 1 - j, i)  -->  A: (i, j)
所以：顺时针旋转90°后，每四个元素为一组，互相覆盖；

4. 对于边长为len的一层来说，元素个数为：4 * (len - 1)；

5. Code(https://leetcode.com/submissions/detail/430511218/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        if (n == 1) {
            return;
        }
        
        // 1. 不难看出，元素(i, j)在顺时针旋转90°后的坐标：(j, n - 1 - i)
        
        // 2. 考虑每一层，层数：n / 2
        // 第一层的左上角坐标：(0, 0)
        int x = 0;
        int y = 0;
        // 第一层的边长：n
        int len = n;
        for (int layer = 1; layer <= n / 2; layer++) {
            // 向右取包含(x, y)在内的len - 1个元素
            for (int offset = 0; offset <= len - 2; offset++) {
                int i = x;
                int j = y + offset;
                int storeI = matrix[i][j];
                matrix[i][j] = matrix[n - 1 - j][i];
                matrix[n - 1 - j][i] = matrix[n - 1 - i][n - 1 - j];
                matrix[n - 1 - i][n - 1 - j] = matrix[j][n - 1- i];
                matrix[j][n - 1 - i] = storeI;
            }
            // 更新下一层的左上角坐标
            x++;
            y++;
            // 更新边长
            len -= 2;
        }
 
        // 3. return
        return;
    }
};
```

### 2、Solution-2：Reverse
1. Code(https://leetcode.com/submissions/detail/430528964/)
AC
时间复杂度：O(n * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        if (n == 1) {
            return;
        }
 
        /*
        * clockwise rotate
        * first reverse up to down, then swap the symmetry 
        * 1 2 3     7 8 9     7 4 1
        * 4 5 6  => 4 5 6  => 8 5 2
        * 7 8 9     1 2 3     9 6 3
        */
 
        /*
        * anticlockwise rotate
        * first reverse left to right, then swap the symmetry
        * 1 2 3     3 2 1     3 6 9
        * 4 5 6  => 6 5 4  => 2 5 8
        * 7 8 9     9 8 7     1 4 7
        */
        
        reverse(matrix.begin(), matrix.end());
        
        for (int i = 0; i <= n - 1; i++) {
            for (int j = i + 1; j <= n - 1; j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
    }
};
```

### 3、总结
1. 对于n * n的方阵来说，in-place旋转：
* 顺（逆）时针旋转0°、360°
不做任何操作；
* 顺时针旋转90°：本题
Solution-1：观察规律，并模拟；
Solution-2：代码简洁，但难以想出来；
* 顺（逆）时针旋转180°：
直接用Solution-2：
将matrix进行reverse；然后对每一行再进行reverse即可；
* 顺时针旋转270°，等价于逆时针旋转90°：
Solution-1：类似于本题，进行观察分析后模拟；
Solution-2：见上述代码中注释；