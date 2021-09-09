---
title: 905.Sort Array By Parity
---

## 一、题意
题目链接: [905.Sort Array By Parity](https://leetcode.com/problems/sort-array-by-parity/)
### 1.Description
【输入】
给定一场长度为n的数组nums。元素都是非负整数。
【输出】
将nums中所有的偶数放在所有的奇数前面。答案有许多种，返回任意满足一种的即可。

### 2.Example
Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

### 3.Corner Case
1. 1 <= A.length <= 5000
2. 0 <= A[i] <= 5000

## 二、题解
### Solution-1：Two Pointers in-place
1.不用额外的空间，就在原始数组上操作。

2.回忆Quick Sort，我们每一轮：选择一个target，然后用left指针指向目标区间最左侧，right指针指向目标区间最右侧。
right指针一直往左移，直到其指向的值比target小。
left指针一直往右移，直到其指向的值比target大。
如果此时left仍然小于right，则交换它们俩的值。
直到left == right为止。

3.对于本题目，也用left和right两个指针，right从后面往前移动，直到找到一个偶数。
left从前面往后移动，直到找到一个奇数，然后交换它们的值即可。
当left == right的时候，nums数组排序完成：所有偶数在所有奇数的前面。

4.缺点：虽然节省了空间，但是修改了input。实施前先确认修改input是否可以。

时间复杂度：O(n)
空间复杂度：O(1)

5.Code(https://leetcode.com/submissions/detail/286997150/)
```C++
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& A) {
        int n = A.size();
        int left = 0;
        int right = n - 1;
        while (left < right) {
            while (left < right && A[right] % 2 == 1) {
                right--;
            }
            while (left < right && A[left] % 2 == 0) {
                left++;
            }
            if (left < right) {
                swap(A[left], A[right]);
            }
        }
        return A;
    }
};
```

### Solution-2：Two Pointers with Extra Space
1.考虑避免修改input，我们新建额外的长度为n的数组ans。

2.left指向ans开始，right指向ans结尾。

3.考虑nums的每一个元素，如果其是偶数，则赋值给ans[left++]，否则赋值给ans[right--]。
考虑n次后，return ans即可。

4.优点：避免了修改input数组。但是使用了额外的space。

5.Code(https://leetcode.com/submissions/detail/286999024/)
```C++
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& A) {
        int n = A.size();
        vector<int> ans(n);
        int left = 0;
        int right = n - 1;
        for (int i = 0; i <= n - 1; i++) {
            if (A[i] % 2 == 0) {
                ans[left++] = A[i];
            } else {
                ans[right--] = A[i];
            }
        }
        return ans;
    }
};
```

