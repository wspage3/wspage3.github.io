---
title: 88.Merge Sorted Array
---

## 一、题意
题目链接: [88.Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
### 1.Description
【输入】
给定两个整数数组nums1和nums2。两个数组都是有序的。
同时给定两个数字m，n。
m代表：nums1中前m个数字是有序的。
n代表：nums2中前n个数字是有序的。nums2的长度为n。
同时nums1的数组长度 >= m + n
【输出】
将nums2数组和nums1数组合并成一个有序的数组，且存在在nums1数组中。

### 2.Example
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]

### 3.Corner Case
1. 若n = 0，不需要任何操作
2. 若m = 0，将nums2拷贝到nums1中即可。

## 二、题解
### Solution-1：Three Pointers
1.使用三个指针p1, p2, cur
cur指向nums1数组的末尾
p1指向nums1的最后一个有序元素
p2指向nums2的最后一个有序元素
当p1和p2均大于等于0时，选出一个较大的元素，放在nums1数组的末尾。

2.判断此时是p1小于0，还是p2小于0。
若p1小于0，说明nums1中所有的有序元素都已经考虑完成了。此时只要将nums2中未考虑的元素依次放入nums1中即可。
若p2小于0，说明nums2中所有的有序元素都已经考虑完成了。此时不需要任何操作，nums1中所有元素都是有序的了。

时间复杂度：O(m + n)  
空间复杂度：O(1)

6.Code(https://leetcode.com/submissions/detail/287061095/)
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int cur = m + n - 1;
        int p1 = m - 1;
        int p2 = n - 1;
        while (p1 >= 0 && p2 >= 0) {
            if (nums1[p1] >= nums2[p2]) {
                nums1[cur--] = nums1[p1--];    
            } else {
                nums1[cur--] = nums2[p2--];
            }
        }
        if (p1 < 0) {
            while (p2 >= 0) {
                nums1[cur--] = nums2[p2--];
            }
        } else {
            // do nothing
        }
        return;
    }
};
```
