---
title: Leetcode-189.Rotate Array
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
---

## 一、题意

### 0、题目链接
[189.Rotate Array](https://leetcode.com/problems/rotate-array/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；
给定一个非负整数k；
【输出】
将nums向右“旋转”k次；
“旋转”：将数组最后一个数移到数组的开头；
【要求】
尝试使用不同解法，至少写出3种；
能否使用原地算法，即仅使用额外O(1)的空间？

### 2、Example
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

<!-- more -->

### 3、Corner Case
1. 1 <= n <= 2 * 10^4
2. k >= 0
3. k == 0时，不做任何操作，返回即可；

## 二、题解

### 0、思考
1. 若nums：[1, 2, 3]，
当k = 1时：[3, 1, 2]
当k = 2时：[2, 3, 1]
当k = 3时：[1, 2, 3]，又回到nums本身
当k = 4时：[3, 1, 2]
当k = 5时：[2, 3, 1]
所以可以先对k进行处理，防止k过大：k = k % n；

### 1、Solution-1：Brute Force
1. 直接模拟，进行k次旋转：每次将[0, n - 2]的元素往右移动一位，将nums[n - 1]移动到nums[0]；

2. Code(https://leetcode.com/submissions/detail/335276009/)
TLE
时间复杂度：O(k * n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        for (int num = 1; num <= k; num++) {
            int last = nums[n - 1];
            for (int j = n - 1; j >= 1; j--) {
                nums[j] = nums[j - 1];
            }
            nums[0] = last;
        }
    }
};
```

### 2、Solution-2：Using Extra Space
1. 先使用clone数组，将nums保存下来；

2. oldIdx对clone数组进行遍历，找到其旋转k次后的newIdx，将clone[oldIdx]赋值给nums[newIdx]即可；

3. Code(https://leetcode.com/submissions/detail/335279427/)
AC
时间复杂度：O(n)
空间复杂度：O(n)
```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        vector<int> clone(nums);
        for (int oldIdx = 0; oldIdx <= n - 1; oldIdx++) {
            int newIdx = (oldIdx + k) % n;
            nums[newIdx] = clone[oldIdx];
        }
    }
};
```

### 3、Solution-3：Using Cyclic Replacements
1. 将本题目想象成：教室中有一排座位，有n个，每个座位上有一个同学；他们都需要往右移动k个位置；

2. 过程：
初始化：
从第0个位置开始（cur = 0）；设置候选人（candidate = nums[0]）；
对候选人分配座位：
计算出候选人的新位置（next = (cur + k) % n）；
为候选人分配座位，同时将被挤出去的该同学设置为候选人（swap(candidate, nums[next])）；
处理下一个候选人（cur = next）；

3. 注意：上述过程，可能会出现环路。
例如：nums = [1, 2, 3, 4, 5, 6]；k = 2；
cur分别处理：0 -> 2 -> 4 -> 0
对于这种情况，我们需要进行检测以及处理；

4. 检测：
cur从start出发，经过若干步，又回到了start，此时就说明出现了环路；

5. 处理：
控制分配座位的次数count为n；
当出现环路时，start后移一位；

6. Code(https://leetcode.com/submissions/detail/335306736/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        
        int count = 0;
        for (int start = 0; count < n; start++) {
            int cur = start;
            int candidate = nums[start];
            do {
                int next = (cur + k) % n;
                swap(nums[next], candidate);
                cur = next;
                count++;
            } while (cur != start);
        }
    }
};
```

### 4、Solution-4：Using Reverse
1. 观察：当对长度为n的nums数组旋转k次后：
nums的后k个数字移动到了nums的开头；
nums的前n - k个数字移动到了nums的结尾；每一个往后移动了k次。
如：nums = [1, 2, 3, 4, 5]； k = 3；
变成：[3, 4, 5, 1, 2]

2. 过程：
首先，将nums进行reverse：[5, 4, 3, 2, 1]
其次，将nums的前k个数字进行reverse：[3, 4, 5, 2, 1]
最后，将nums的后(n - k)个数字进行reverse：[3, 4, 5, 1, 2]

3. Code(https://leetcode.com/submissions/detail/259839316/)
AC
时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size();
        reverse(nums.begin(), nums.end());
        reverse(nums.begin(), nums.begin() + k);
        reverse(nums.begin() + k, nums.end());
    }
};
```