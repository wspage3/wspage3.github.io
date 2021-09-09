---
title: 922.Sort Array By Parity II
---

## 一、题意
题目链接: [922.Sort Array By Parity II](https://leetcode.com/problems/sort-array-by-parity-ii/)
### 1.Description
【输入】
给定一场长度为n的数组nums。元素都是非负整数。n为偶数。
其中一半元素是奇数，一半元素是偶数。
【输出】
将nums中index为偶数的位置放置偶数，index为奇数的位置放置奇数。
即：偶奇偶奇偶奇偶奇
答案有许多种，返回任意满足一种的即可。

### 2.Example
Input: [4,2,5,7]
Output: [4,5,2,7]
Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.

### 3.Corner Case
1. 2 <= A.length <= 20000
2. A.length % 2 == 0
3. 0 <= A[i] <= 1000

## 二、题解
### Solution-1：Two Pointers in-place
1.不用额外的空间，就在原始数组上操作。

2.和上一题目一样，right从后往前找一个偶数。
left从前往后找一个奇数。
然后交换它们俩的值。
两个指针每次都是移动2步。
注意边界值的考虑即可。

3.缺点：虽然节省了空间，但是修改了input。实施前先确认修改input是否可以。

时间复杂度：O(n)
空间复杂度：O(1)

5.Code(https://leetcode.com/submissions/detail/287006385/)
```C++
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& A) {
        int n = A.size();
        int left = 0;
        int right = n - 1;
        while (left <= n - 2 && right >= 1) {
            while (right >= 1 && A[right] % 2 == 1) {
                right -= 2;
            }
            while (left <= n - 2 && A[left] % 2 == 0) {
                left += 2;
            }
            if (left <= n - 2 && right >= 1) {
                swap(A[left], A[right]);
            }
        }
        return A;
    }
};
```

### Solution-2：Two Pointers with Extra Space
1.考虑避免修改input，我们新建额外的长度为n的数组ans。

2.left指向0，right指向1。

3.优点：避免了修改input数组。但是使用了额外的space。

4.Code(https://leetcode.com/submissions/detail/287001444/)
```C++
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& A) {
        int n = A.size();
        vector<int> ans(n);
        int left = 0;
        int right = 1;
        for (int i = 0; i <= n - 1; i++) {
            if (A[i] % 2 == 0) {
                ans[left] = A[i];
                left += 2;
            } else {
                ans[right] = A[i];
                right += 2;
            }
        }
        return ans;
    }
};
```

