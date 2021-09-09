---
title: Leetcode-378.Kth Smallest Element in a Sorted Matrix
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Heap
- Priority Queue
- Binary Search
---

## 一、题意

### 0、题目链接
[378.Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

### 1、Description
【输入】
给定一个n * n的二维数组matrix；matrix的每行每列都是升序的；
给定一个整数k；
【输出】
输出matrix中第k小的数字；
此处第k小是指将matrix中所有元素从小到大排序后，从前到后数的第k个数字；而非第k个不重复的数字；
【要求】
无；

### 2、Example
matrix = 
[
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.

<!-- more -->

### 3、Corner Case
1. k合法：1 <= k <= n * n；
2. n == 0时，不考虑；
3. n == 1时，返回matrix[0][0]即可；
4. 考虑n >= 2的情况；

## 二、题解

### 0、思考
1. 对于Kth的问题，考虑是否可以使用Heap来解决；

### 1、Solution-1：Max Heap
1. 要在n * n个数字中，寻找第k小的数字。

2. 维护一个大小为k的最大堆。

3. 将n * n个数字都加入到最大堆中，若最大堆的大小超过了k，则去除堆顶（较大）元素。

4. n * n次循环后，最大堆中的k个数字就是matrix中最小的那k个数字了。其中堆顶的就是第k小的数字。

5. Code(https://leetcode.com/submissions/detail/324981074/)
AC
时间复杂度：O(n * n * log k)  
空间复杂度：O(k)
```C++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        if (n == 0) {
            return -1; // invalid testcase
        }
        if (n == 1) {
            return matrix[0][0];
        }
        // Solution-1: 将matrix中所有的元素放入大小为k的大顶堆中。堆顶元素即为第k小的元素。
        // 缺点：完全没有利用matrix数组按行按列都是有序的特性。
        priority_queue<int> maxHeap;
        for (int i = 0; i <= n - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                maxHeap.push(matrix[i][j]);
                if (maxHeap.size() > k) {
                    maxHeap.pop();
                }
            }
        }
        return maxHeap.top();
    }
};
```

### 2、Solution-2：Min Heap
1. 类似于023.Merge k Sorted Lists；

2. 可以将n * n的matrix想象成n个链表（水平的），每个链表的长度为n，而且从左到右是升序的。

3. 保持minHeap的大小为L：min(k, n)。

4. 首先：将matrix[0][0]、matrix[1][0]...matrix[L - 1][0]放入minHeap中；

5. 然后：循环k - 1次，每次将堆顶元素挤出去，再加入其后继结点。

6. 最后：堆顶元素就是matrix中第k小的元素了。

7. Code(https://leetcode.com/submissions/detail/331262835/)
AC
时间复杂度：O(k * log L) // L = min(k, n)
空间复杂度：O(L)
```C++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        if (n == 0) {
            return -1; // invalid testcase
        }
        if (n == 1) {
            return matrix[0][0];
        }
        // Solution-2: 最小堆 + 多路归并
        // 类似于题目：023.Merge k Sorted Lists
        
        // Step-0
        // 由于将元素挤出去的同时，需要插入其在matrix中同一行下一列的元素。所以记录下元素当前的坐标。
        // pair<int, pair<int, int>>
        // { value, {x, y} }
        
        // Step-1
        priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> minHeap;
        for (int i = 0; i <= min(k - 1, n - 1); i++) {
            minHeap.push({matrix[i][0], {i, 0}});
        }
        
        // Step-2
        for (int num = 1; num <= k - 1; num++) {
            int curX = minHeap.top().second.first;
            int curY = minHeap.top().second.second;
            // 挤出去
            minHeap.pop();
            // 如果该行没有更多的元素了，则不加进去了。
            if (curY == n - 1) {
                continue; 
            }
            // 加进来
            minHeap.push({matrix[curX][curY + 1], {curX, curY + 1}});
        }
        
        // Step-3
        return minHeap.top().first;
    }
};
```

### 3、Solution-3：Binary Search
1. 由于n * n大小的matrix每行每列，都是升序排列好的。所以对于任意matrix[i][j]，其取值都在范围[matrix[0][0], matrix[n - 1][n - 1]]内。

2. 同时，对于一个m * n的matrix，如果其每行每列都是有序的，我们可以在O(m + n)的时间内：判断target是否存在；或者统计大于（小于）target的元素个数。
参考题目：
240-Search a 2D Matrix II
1351-Count Negative Numbers in a Sorted Matrix

3. 根据以上两点，我们可以对值域[matrix[0][0], matrix[n - 1][n - 1]]进行二分搜索，找出第一个满足条件的数字ans，ans就是我们要求的答案。

4. 满足的条件：使得search(target) >= k。

5. search(target)：求出matrix中小于等于target的元素个数。可见target越大，返回值越大。

6. 理解：
如果search(target) < k，即matrix中小于等于target的数字的个数小于k，那么说明：当前target太小啦，ans > target，需要在[mid + 1, right)中搜索；
如果search(target) >= k，即matrix中小于等于target的数字的个数大于等于k，那么说明：当前target已经满足条件了，但要找第一个满足条件的，需要在[left, mid)中搜索；

7. Code(https://leetcode.com/submissions/detail/331286557/)
AC
时间复杂度：O(n * log L) // 其中L代表：matrix中左上角元素与右下角元素的差值 
空间复杂度：O(1)
```C++
class Solution {
private:
    int search(vector<vector<int>>& matrix, int n, int target) {
        // 在行、列都有序（升序）的matrix中，求出小于等于target的元素个数。
        int r = n - 1;
        int c = 0;
        // cnt代表大于target的元素个数。
        int cnt = 0;
        while (r >= 0 && c <= n - 1) {
            if (matrix[r][c] <= target) {
                c++;
            } else {
                cnt += (n - c);
                r--;
            }
        }
        // 小于等于target的元素个数。
        return n * n - cnt;
    }
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        if (n == 0) {
            return -1; // invalid testcase
        }
        if (n == 1) {
            return matrix[0][0];
        }
        
        // Solution-3: Binary Search(值域二分，非索引二分)      
        
        // Q：为什么一定保证left这个值域内的值一定在matrix中出现呢？
        // A：left在进行赋值的时候，其左边[left, mid]一定不存在满足要求的元素。此时将left = mid + 1，此处仅仅是进行尝试。
        //    若其cnt仍然小于k，我们继续在[mid + 1, right]中搜索。因为cnt()函数在值域区间是递增的，一定会在某个时候 >= k。
        //    此时不断逼近，使得[left, right]不断变小，且最终靠近第一个满足要求的值的附近。
        //    继续在[mid + 1, right]中搜索，此处二分的同时，进行了暴力枚举，每次递增一个数字，使得最终的left一定是在matrix中出现的。
        
        int left = matrix[0][0];
        int right = matrix[n - 1][n - 1] + 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 求出matrix中小于等于mid的元素个数。
            int cnt = search(matrix, n, mid);
            if (cnt >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        // 此时一定有left == right，且可能的取值范围为：[matrix[0][0], matrix[n - 1][n - 1] + 1]。
        // 如果left == matrix[n - 1][n - 1] + 1，说明matrix中不存在满足条件的元素。
        // 即对于任一matrix[i][j]，search(matrix[i][j]) < k都成立。
        // 而题目中保证了：1 <= k <= n * n。k最大为n * n。而search(matrix[n - 1][n - 1]) == n * n。
        // 所以分析可得，left不可能等于matrix[n - 1][n - 1] + 1。
        return left;
    }
};
```