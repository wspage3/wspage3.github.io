---
title: 704.Binary Search
---

## 一、题意
题目链接: [704.Binary Search](https://leetcode.com/problems/binary-search/)
### 1.Description
给定一个长度为n的数组，该数组是升序排列的，中间没有有重复的元素；
给定一个整数target；
查找target是否在数组中，在的话，返回其索引。
不存在的话，返回-1即可；
### 2.Example
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
### 3.Corner Case
n: [1, 10000]
nums[i]: [-9999,9999]

## 二、题解
### Solution-1：Binary Search
1.二分查找，每次取mid，然后判断nums[mid]的值是否是target，如果是的话，说明当前mid就是target的索引。
如果target大于nums[mid]，则在[mid + 1, right]中搜。
如果target小于nums[mid]，则在[left, mid - 1]中搜。

2.在区间[0, n - 1]上搜索。开始left = 0, right = n - 1。
每次考虑nums[mid]，如果其不是答案，则继续在mid左边或者mid右边查找。

3.循环判断：left <= right，这样可以保证[0, n - 1]上的每个元素都能被考虑到。
最终循环结束的时候，一定有left = right + 1。
如果循环结束了还没有找到target，说明数组中没有target。

时间复杂度：O(log n)
空间复杂度：O(1)

3.Code(https://leetcode.com/submissions/detail/285484850/)

### Solution-2：Binary Search with minimum index
1.核心思想还是二分查找。我们尝试找target最小的index（如果target有多个的话）

2.假设某一时刻，在区间[left, right)上查找。
继续取mid，判断nums[mid]和target的大小关系。
此时区间[left, right)被分成：[left, mid), nums[mid], [mid + 1, right)三部分。
我们通过判断nums[mid]和target的大小关系来决定下次搜索在上面范围内：
如果target > nums[mid]，说明target的最小索引一定在[mid + 1, right)这个范围上。
如果target < nums[mid]，说明target的最小索引一定在[left, mid)这个范围上。
如果target == nums[mid]，说明target的最小索引要么就在mid位置；要么在mid位置之前；
这种情况下，我们继续在[left, mid)上寻找，如果这个范围内已经没有和target相等的数字了，那么答案就是mid。
如果这个范围内又找到了target，说明target的最小索引在这个范围内。

举个例子：
[1, 2, 2, 3], target = 2
在范围[0, 4)上搜索，mid = 2, nums[mid] == target。此时将right = mid = 2。
在范围[0, 2)上搜索，mid = 1, nums[mid] == target。此时将right = mid = 1。
在范围[0, 1)上搜索，mid = 0, target > nums[0]。此时left = mid + 1 = 1。
left == right == 1，此时循环结束。
可以看出，1就是target的最小索引。

[1, 1, 2, 3], target = 2
在范围[0, 4)上搜索，mid = 2, nums[mid] == target。此时将right = mid = 2。
在范围[0, 2)上搜索，mid = 1, target > nums[1]。此时left = mid + 1 = 2。
left == right == 2，此时循环结束。
可以看出，2就是target的最小索引。

3.以上两个例子都是nums中包含target的情况，下面看nums中不包含target的情况。
[1, 3, 5, 7], target = 0
在范围[0, 4)上搜索，mid = 2, target < nums[mid]。此时将right = mid = 2。
在范围[0, 2)上搜索，mid = 1, target < nums[mid]。此时将right = mid = 1。
在范围[0, 1)上搜索，mid = 0, target < nums[mid]。此时将right = mid = 0。
left == rihgt == 0，此时循环结束。
再次判断：【如果这个位置是target，则是target的最小索引；】
否则的话，说明target不在nums中。
而nums[0] = 1，而不是target，说明target不存在于nums中。

[1, 3, 5, 7], target = 10
在范围[0, 4)上搜索，mid = 2, target > nums[mid]。此时将left = mid + 1 = 3。
在范围[3, 4)上搜索，mid = 3, target > nums[mid]。此时将left = mid + 1 = 4。
left == rihgt == 4，此时循环结束。
再次判断：【如果这个位置是target，则是target的最小索引；】
而此时的left(right)已经代表不了数组的index了，说明target不在nums中。
即：【若left(right) == n，则target不在nums中】

[1, 3, 5, 7], target = 6
在范围[0, 4)上搜索，mid = 2, target > nums[mid]。此时将left = mid + 1 = 3。
在范围[3, 4)上搜索，mid = 3, target < nums[mid]。此时将rihgt = mid = 3。
left == right == 3，此时循环结束。
再次判断：【如果这个位置是target，则是target的最小索引；】
否则的话，说明target不在nums中。
而nums[3] = 7，而不是target，说明target不存在于nums中。


时间复杂度：O(log n)
空间复杂度：O(1)

3.Code(https://leetcode.com/submissions/detail/285596680/)

### Solution-2：Binary Search with minimum index
1.核心思想还是二分查找。我们尝试找target最大的index（如果target有多个的话）

2.假设某一时刻，在区间[left, right)上查找。
继续取mid，判断nums[mid]和target的大小关系。
此时区间[left, right)被分成：[left, mid), nums[mid], [mid + 1, right)三部分。
我们通过判断nums[mid]和target的大小关系来决定下次搜索在上面范围内：
如果target > nums[mid]，说明target的最大索引一定在[mid + 1, right)这个范围上。
如果target < nums[mid]，说明target的最大索引一定在[left, mid)这个范围上。
如果target == nums[mid]，说明target的最大索引要么就在mid位置；要么在mid位置之后；
这种情况下，我们继续在[mid + 1, right)上寻找:
如果这个范围内已经没有和target相等的数字了，那么答案就是mid。
如果这个范围内又找到了target，说明target的最大索引在这个范围内。

举个例子：
[1, 2, 2, 3], target = 2
在范围[0, 4)上搜索，mid = 2, nums[mid] == target。此时将left = mid + 1 = 3。
在范围[3, 4)上搜索，mid = 3, nums[mid] > target。此时将right = mid = 3。
left == right == 3，此时循环结束。
可以看出，【3 - 1】就是target的最大索引。

[1, 1, 2, 3], target = 2
在范围[0, 4)上搜索，mid = 2, nums[mid] == target。此时将left = mid + 1 = 3。
在范围[3, 4)上搜索，mid = 3, nums[mid] > target。此时right = mid = 3。
left == right == 3，此时循环结束。
可以看出，【3 - 1】就是target的最大索引。

3.以上两个例子都是nums中包含target的情况，下面看nums中不包含target的情况。
[1, 3, 5, 7], target = 0
在范围[0, 4)上搜索，mid = 2, target < nums[mid]。此时将right = mid = 2。
在范围[0, 2)上搜索，mid = 1, target < nums[mid]。此时将right = mid = 1。
在范围[0, 1)上搜索，mid = 0, target < nums[mid]。此时将right = mid = 0。
left == right == 0，此时循环结束。
再次判断：【如果这个位置的前一个位置是target，则是target的最大索引；】
否则的话，说明target不在nums中。
而left(right)的前一个是-1，已经不能表示index了。说明target不存在。
即：【若left(right) == 0，则target不在nums中】

[1, 3, 5, 7], target = 10
在范围[0, 4)上搜索，mid = 2, target > nums[mid]。此时将left = mid + 1 = 3。
在范围[3, 4)上搜索，mid = 3, target > nums[mid]。此时将left = mid + 1 = 4。
left == right == 4，此时循环结束。
再次判断：【如果这个位置的前一个位置是target，则是target的最大索引；】
否则的话，说明target不在nums中。
而nums[3]的值为7，说明target不在nums中。

[1, 3, 5, 7], target = 2
在范围[0, 4)上搜索，mid = 2, target < nums[mid]。此时将right = mid = 2。
在范围[0, 2)上搜索，mid = 1, target < nums[mid]。此时将right = mid = 1。
在范围[0, 1)上搜索，mid = 0, target > nums[mid]。此时将left = mid + 1 = 1。
left == right == 1，此时循环结束。
再次判断：【如果这个位置的前一个位置是target，则是target的最大索引；】
否则的话，说明target不在nums中。
而nums[0]的值为1，说明target不在nums中。


时间复杂度：O(log n)
空间复杂度：O(1)

3.Code(https://leetcode.com/submissions/detail/285657074/)
