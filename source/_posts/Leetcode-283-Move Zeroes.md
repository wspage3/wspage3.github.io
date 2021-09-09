---
title: 283.Move Zeroes
---

## 一、题意
题目链接: [283.Move Zeroes](https://leetcode.com/problems/move-zeroes/)
### 1.Description
【输入】
给定一个长度为n的整数数组。
【输出】
将其中出现的‘0’都移动到末尾，同时其他非0的元素的相对顺序不变。

### 2.Example
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]

### 3.Corner Case
1. 就地操作，O(1)的space complexity
2. 使得所有操作的次数尽可能地少

## 二、题解
### Solution-1：Brute Force
1.cnt代表0的个数。
i从前往后遍历：
如果在位置nums[i]发现一个‘0’。
cnt++
将[i + 1, n - cnt]上面所有的数字向前移动一次。
将nums[n - cnt]赋值为0。

2.直到i = n - cnt为止。

时间复杂度：O(n ^ 2)
空间复杂度：O(1)

6.Code(https://leetcode.com/submissions/detail/287111624/)
```C++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();
        int cnt = 0;
        for (int i = 0; i <= n - 1 - cnt;) {
            if (nums[i] == 0) {
                cnt++;
                int cur = i;
                while (cur <= n - 1 - cnt) {
                    nums[cur] = nums[cur + 1];
                    cur++;
                }
                nums[cur] = 0;
            } else {
                i++;
            }
        }
        return;
    }
};
```

### Solution-2：Two Pointers
1.指针j从0开始，指向第一个为0的位置。

2.指针i从j + 1开始，指向第一个非0的位置。
此时将nums[i]赋值给nums[j]。
同时i，j往后移一位。

3.最后，将[j, n - 1]内所有的数字赋值为0。

4.j的含义：[0, j - 1]的数字都是非0的。
i的含义：考虑所有可能非0的位置。

5.举个例子：
[0,1,0,3,12]  n = 5
j = 0

i = 1 : nums[0] = 1, j = 1
i = 2 : 
i = 3 : nums[1] = 3, j = 2
i = 4 : nums[2] = 12, j = 3

j = 3 : nums[3] = 0
j = 4 : nums[4] = 0

nums: [1,3,12,0,0]

时间复杂度：O(n)
空间复杂度：O(1)

6.Code(https://leetcode.com/submissions/detail/287504746/)
```C++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();
        int j = 0;
        while (j <= n - 1 && nums[j] != 0) {
            j++;
        }
        for (int i = j + 1; i <= n - 1; i++) {
            if (nums[i] != 0) {
                nums[j++] = nums[i];
            }
        }
        while (j <= n - 1) {
            nums[j++] = 0;
        }
        return;
    }
};
```






