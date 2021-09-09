---
title: 448.Find All Numbers Disappeared in an Array 
---

## 一、题意
题目链接: [448.Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
### 1.Description
【输入】
给定长度为n的数组。其中所有元素的取值范围都在[1, n]中间。
其中有些元素出现了2次，有些元素没有出现。
【输出】
找出所有没有出现的元素，将其用数组的形式返回。

### 2.Example
Input:
[4,3,2,7,8,2,3,1]
Output:
[5,6]

### 3.Corner Case
1. 要求O(n)的time complexity
2. 要求O(1)的space complexity
3. 判断n是否为0，若为0返回空数组即可。

## 二、题解
### Solution-1：标记法
1.由于元素取值范围都在[1, n]中间，
所以任意nums[i] - 1都可以作为nums的index。

2.举个例子：
index: [0, 1, 2, 3, 4, 5, 6, 7]
nums:  [4, 3, 2, 7, 8, 2, 3, 1]
遍历一次，将nums[nums[i] - 1]由正数变为负数；若已经是负数，则不处理：
nums:  [-4, -3, -2, -7, 8, 2, -3, -1]
此时发现index为4和5的数字仍然是正数。说明数组中不存在5和6这两个数字。

时间复杂度：O(n)
空间复杂度：O(1) //不包含返回的数组ans

3.Code(https://leetcode.com/submissions/detail/287116928/)
```C++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> ans;
        int n = nums.size();
        for (int i = 0; i <= n - 1; i++) {
            int index = abs(nums[i]) - 1;
            if (nums[index] > 0) {
                nums[index] = -nums[index];
            }
        }
        for (int i = 0; i <= n - 1; i++) {
            if (nums[i] > 0) {
                ans.push_back(i + 1);
            }
        }
        return ans;
    }
};
```
