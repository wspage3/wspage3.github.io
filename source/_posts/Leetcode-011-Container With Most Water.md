---
title: 110.Container With Most Water
---

## 一、题意
题目链接: [110.Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
### 1.Description
【输入】
给定长度为n的数组。其中元素都是非负数。
height[i]代表：位置i处有一根高度为height[i]的杆子。
【输出】
任意选择两根杆子，求出以这两根杆子、地面，所能构成的最大矩形面积。

### 2.Example
Input: [1,8,6,2,5,4,8,3,7]
Output: 49

### 3.Corner Case
1. height[i]非负。
2. n >= 2

## 二、题解
### Solution-1：Brute Force
1. 枚举所有可能的两根杆子组合情况，求出max值。
对于i, j两根杆子来说，高度为min(height[i], height[j])，长度为(j - i)。

时间复杂度：O(n ^ 2)
空间复杂度：O(1)

2.Code(https://leetcode.com/submissions/detail/287284450/)
```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();
        int ans = INT_MIN;
        for (int i = 0; i <= n - 2; i++) {
            for (int j = i + 1; j <= n - 1; j++) {
                int temp = (j - i) * min(height[i], height[j]);
                ans = max(ans, temp);
            }
        }
        return ans;
    }
};
```

### Solution-2：Two Pointers
1. 使用两个指针：left指向0，right指向n - 1。

2. 比较height[left]和height[right]的大小：
若height[left] < height[right]，则记h为height[left]，left++，直到nums[left] > h。
若height[left] > height[right]，则记h为height[right]，right--，直到nums[right] > h。
若height[left] == height[right]，则记h为height[left]（height[right]）。left和right同时往中间移动。直到两者都找到比h大的值为止。
这样，当left和right相遇的时候，最优解就已经计算出来了。

3. 参考：
剪枝理解：(https://leetcode.com/problems/container-with-most-water/discuss/6099/Yet-another-way-to-see-what-happens-in-the-O(n)-algorithm)
证明思路：(https://leetcode.com/problems/container-with-most-water/discuss/6090/Simple-and-fast-C%2B%2BC-with-explanation)

时间复杂度：O(n)
空间复杂度：O(1)

4.Code(https://leetcode.com/submissions/detail/287300934/)
```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();
        int ans = INT_MIN;
        int left = 0;
        int right = n - 1;
        while (left < right) {
            int h = min(height[left], height[right]);
            ans = max(ans, h * (right - left));
            while (left < right && height[left] <= h) {
                left++;
            }
            while (left < right && height[right] <= h) {
                right--;
            }
        }
        return ans;
    }
};
```


