---
title: 442.Find All Duplicate in an Array
---

## 一、题意
题目链接: [442.Find All Duplicate in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
### 1.Description
【输入】
给定一个长度为n的整数数组。
其中所有的元素取值都是范围[1, n]的。
但其中有些元素出现了两次，其它的元素出现了一次。
【输出】
找出所有出现两次的元素。

### 2.Example
Input:
[4,3,2,7,8,2,3,1]
Output:
[2,3]

### 3.Corner Case
1.n <= 1, 不存在题目中的情况。考虑n >= 2的情况。
2.要求O(1)的space。
3.要求O(n)的time。

## 二、题解
### Solution-1：做标记
1.由于nums数组中的元素都在范围[1, n]中。
所以对于任意nums[i] - 1，都可以作为nums的index。

2.我们对于所有i: [0, n - 1]，将nums[i] - 1作为index。
将nums[index]的值替换为其值的相反数。

3.假设某个数字x出现了两次，那么(x - 1)作为nums的index，将同样会出现两次。
第一次：将nums[x - 1]的值由正数变换为负数。
第二次：发现nums[x - 1]已经是负数了。说明：(x - 1)这个索引在之前已经出现过了。即：x是一个答案。

4.由于将nums[index]会变换为其相反数，所以为保证index的合法性，可以：index = abs(nums[i]) - 1;

时间复杂度：O(n)
空间复杂度：O(1)

2.Code(https://leetcode.com/submissions/detail/286253289/)
