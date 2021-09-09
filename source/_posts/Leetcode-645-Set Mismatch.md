---
title: 645.Set Mismatch
---

## 一、题意
题目链接: [645.Set Mismatch](https://leetcode.com/problems/set-mismatch/)
### 1.Description
【输入】
给定一个长度为n的整数数组。
其中所有的元素取值都是范围[1, n]的。
但其中某个元素出现了两次，导致另外一个元素没有出现。
【输出】
找出这两个元素：出现两次的在前，没有出现的在后。
以数组的形式返回。

### 2.Example
Input: nums = [1,2,2,4]
Output: [2,3]

### 3.Corner Case
1.n: [2, 10000]
2.nums数组无序。

## 二、题解
### Solution-1：做标记
1.根据【Leetcode-442-Find All Duplicates in an Array】的启示：
对于一个长度为n的数组nums，其所有nums[i]都在[1, n]区间内，我们可以通过将nums[nums[i] - 1]置为负数（相反数）的方法，找出所有出现2次的元素。
本题目中保证了只有一个元素出现了两次，所以找到它后break即可。O(n)的time complexity。

2.同时根据【Leetcode-268-Missing Number】的启示：
对于一个长度为n的数组nums，其所有nums[i]都在[0, n]区间内，且不重复。
我们可以找到那个缺失的元素x。
通过：将0, 1, 2, 3, 4, ..., n, nums[0], nums[1], nums[2], ..., nums[n - 1]这(n + 1 + n)个元素都异或起来。
缺失元素x就是这个异或的值。O(n)的time complexity。

3.对于本题目，长度为n的nums数组，所有元素取值范围都在[1, n]。
某个元素a出现了两次，某个元素b出现了0次。即：[1, 2, 3, 4, ..., a, a, ..., b - 1, b + 1, ..., n]
根据1，可以找到a的值。存在res[0]中。res[0] = a;
根据2，将nums中所有的元素和[1, n]中所有的元素异或起来的值存到res[1]中。res[1] = a ^ b;
然后再将res[1] = res[1] ^ res[0]即可，即找到了b的值。

时间复杂度：O(n)
空间复杂度：O(1)

2.Code(https://leetcode.com/submissions/detail/286256728/)
