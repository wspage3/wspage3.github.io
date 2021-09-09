---
title: 611.Valid Triangle Number
---

## 一、题意
题目链接: [611.Valid Triangle Number](https://leetcode.com/problems/valid-triangle-number/)
### 1.Description
【输入】
给定一个长度为n的数组。数组元素都是非负整数。
【输出】
计算存在多少个三元组，使得这个三元组可以组成三角形。

### 2.Example
Input: [2,2,3,4]
Output: 3
Explanation:
Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3

### 3.Corner Case
1. n <= 1000 
   n小于3时直接返回0
2. 0 <= nums[i] <= 1000

## 二、题解
### Solution-1：Brute Force
1.枚举所有的可能性：
i: [0, n - 3]
j: [i + 1, n - 2]
k: [j + 1, n - 1]

2.对于每一种可能的情况，判断nums[i], nums[j], nums[k]是否能够组成三角形

时间复杂度：O(n ^ 3)
空间复杂度：O(1)
结果：TLE

6.Code(https://leetcode.com/submissions/detail/286570851/)
```C++
class Solution {
private:
    bool check(int a, int b, int c) {
        if (a + b <= c || a + c <= b || b + c <= a) {
            return false;
        }
        return true;
    }
public:
    int triangleNumber(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        if (n < 3) {
            return ans;
        }
        for (int i = 0; i <= n - 3; i++) {
            for (int j = i + 1; j <= n - 2; j++) {
                for (int k = j + 1; k <= n - 1; k++) {
                    if (check(nums[i], nums[j], nums[k])) {
                        ans++;
                    }
                }
            }
        }
        return ans;
    }
};
```

### Solution-2：Sort + Three Pointers
1.先将nums数组从小到大排序。

2.枚举所有的最长边，i: [2, i - 1]

3.对于任意一个i，初始化左右指针：left = 0; right = i - 1;
循环条件：left < right

当nums[left] + nums[right] <= nums[i]的时候，说明此时的left，right构不成三角形。
需要它们两个的和继续变大才有可能，所以left++;
当nums[left] + nums[right] > nums[i]的时候，说明此时已经可以构成三角形了。
同时说明：以right结尾，[left, right - 1]都可以构成三角形。因为数组升序排列。计数器累加(right - left)次。
同时right向前移动，看以right - 1是否能够继续构成三角形......

4.这样的话，每次不是left++，就是right--，最后两个指针一定会相见。
对于每一个nums[i]，需要O(n)的time，来计算出以nums[i]为最长边所能构成的情况数目。
所以总的时间复杂度为O(n ^ 2)。

时间复杂度：O(n ^ 2)
空间复杂度：O(1)
结果：TLE

6.Code(https://leetcode.com/submissions/detail/286586836/)
```C++
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        if (n < 3) {
            return ans;
        }
        sort(nums.begin(), nums.end());
        for (int i = 2; i <= n - 1; i++) {
            int left = 0;
            int right = i - 1;
            while (left < right) {
                if (nums[left] + nums[right] <= nums[i]) {
                    left++;
                } else {
                    ans += (right - left);
                    right--;
                }
            }
        }
        return ans;
    }
};
```