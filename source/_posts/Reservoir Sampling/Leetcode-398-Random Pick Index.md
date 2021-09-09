---
title: Leetcode-398.Random Pick Index
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Hash Table
- Reservoir Sampling
---

## 一、题意

### 0、题目链接
[398.Random Pick Index](https://leetcode.com/problems/random-pick-index/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；数字在nums中可能不止出现一次；
给定一个整数target，target一定在nums中。
【输出】
随机输出一个index，使得：nums[index] == target；
【要求】
使得满足条件的每一个index被选择的概率都相等；
如果数组非常大，且不使用额外空间（除了保存nums之外的内存，视为额外）。如何解决？

### 2、Example
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. 
// Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);

<!-- more -->

### 3、Corner Case
1. 假设题目保证n > 0；

## 二、题解

### 0、思考
1. 类似于382.Linked List Random Node；

### 1、Solution-1：Hash Table
1. 初始化：遍历nums，对于每个数字，使用一个一维数组记录其所有的位置。同时使用哈希表来进行关联。

2. pick(target)：找到target对应的一维数组，根据其大小，生成一个随机数。并直接读取该索引，返回即可。

3. 缺点：需要使用额外的空间；初始化需要O(n)的时间复杂度；

4. 优点：每次pick只需要O(1)的时间复杂度；

5. Code(https://leetcode.com/submissions/detail/321544698/)
AC
初始化时间复杂度：O(n)
pick时间复杂度：O(1)
空间复杂度：O(n)
```C++
class Solution {
private:
    unordered_map<int, vector<int>> dict;
public:
    Solution(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i <= n - 1; i++) {
            dict[nums[i]].push_back(i);
        }
    }
    
    int pick(int target) {
        int len = dict[target].size();
        int r = rand() % len;
        return dict[target][r];
    }
};
```

### 2、Solution-2：Reservoir Sampling
1. 初始化：将nums保存起来即可。

2. pick(target)：利用“蓄水池抽样”算法，对nums遍历一次；

3. 缺点：pick的时间复杂度需要O(n)，对于多次pick来说，不友好；

4. 优点：没有使用额外的空间；初始化时间复杂度为O(1)；

5. Code(https://leetcode.com/submissions/detail/321553222/)
AC
初始化时间复杂度：O(1)
pick时间复杂度：O(n)
空间复杂度：O(1)
```C++
class Solution {
private:
    vector<int> v;
public:
    Solution(vector<int>& nums) {
        srand(time(0));
        v = nums;
    }
    
    int pick(int target) {
        int ans;
        int num = 0;
        int n = v.size();
        for (int i = 0; i <= n - 1; i++) {
            if (v[i] != target) {
                continue;
            } 
            if (num == 0) {
                ans = i;
                num++;
                continue;
            }
            num++; 
            if (rand() % num == 0) {
                ans = i;
            }
        }
        return ans;
    }
};
```

