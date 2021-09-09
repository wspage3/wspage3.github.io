---
title: Leetcode-090.Subsets II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
- Subset
---

## 一、题意

### 0、题目链接
[090.Subsets II](https://leetcode.com/problems/subsets-ii/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；
nums性质：可能有重复元素；
【输出】
求出nums的所有【子集】（包括空集和nums本身）；
每一个【子集】用一个一维数组表示；最后返回一个二维数组；
【要求】
无；

### 2、Example
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

<!-- more -->

### 3、Corner Case
1. n == 0时，返回空二维数组；
2. 考虑n为正数的情况；

## 二、题解

### 0、思考
1. 类似于078.Subsets：
078中，nums无重复元素；
本题中，nums中可能存在重复元素；

2. 对于n个元素的nums来说，其子集个数为：2 ^ n；

3. 判断重复：
[1,2,2]
[[],[1],[2(idx=1)],[1,2(idx=1)],[2(idx=2)],[1,2(idx=2)],[2,2],[1,2,2]]
本题要求这种重复情况只输出其中一个；

### 1、Solution-1：Backtracking
1. 当有重复元素时，需要考虑如何去重；

2. 去重策略：
首先，将nums进行从小到大排序；
然后，判断当前结点node：若node不是其父亲的第1个儿子first，则进行判断，看它的值是否等于前一个元素的值；

3. Code(https://leetcode.com/submissions/detail/423568252/)
AC
时间复杂度：O(n * num) // num = 2 ^ n；一共2^n个状态，每种状态需要O(n)的时间来构造子集。
空间复杂度：O(n)
```C++
class Solution {
private:
    void backtracking(int start, vector<int>& nums, vector<int>& sub, vector<vector<int>>& ans) {
        ans.push_back(sub);
        int n = nums.size();
        for (int i = start; i <= n - 1; i++) {
            if (i > start && nums[i] == nums[i - 1]) {
                continue;
            }
            sub.push_back(nums[i]);
            backtracking(i + 1, nums, sub, ans);
            sub.pop_back();
        }
    }
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        if (nums.size() == 0) {
            return {{}};
        }
        
        sort(nums.begin(), nums.end());
        vector<int> sub;
        vector<vector<int>> ans;
        backtracking(0, nums, sub, ans);
        return ans;
    }
};
```

### 2、Solution-2：Iterative
1. 举例：
nums = [1, 1, 2, 2]
初始化：ans = [[]];
i = 0：ans = [[], [1]]
i = 1：ans = [[], [1], [1, 1]]
i = 2：ans = [[], [1], [1, 1], [2], [1, 2], [1, 1, 2]]
i = 3：ans = [[], [1], [1, 1], [2], [1, 2], [1, 1, 2], [2, 2], [1, 2, 2], [1, 1, 2, 2]]

2. Code(https://leetcode.com/submissions/detail/351181659/)
AC
时间复杂度：O(n * num) // num = 2 ^ n；一共2^n个状态，每种状态需要O(n)的时间来构造子集。
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return {{}};
        }
        
        sort(nums.begin(), nums.end());
        
        vector<vector<int>> ans = {{}};
        int size = 1;
        for (int i = 0; i <= n - 1; i++) {
            int cur = nums[i];
            int start = (i >= 1 && nums[i] == nums[i - 1]) ? size : 0;
            size = ans.size();
            for (int j = start; j <= size - 1; j++) {
                ans.push_back(ans[j]);
                ans.back().push_back(cur);
            }
        }
        
        return ans;
    }
};
```

### 3、总结
1. 回溯：
递归树的每个结点都是一个答案；
[1,2]和[2,1]是等价的，用“不走回头路”实现；
nums可能有重复元素；去重策略：对nums排序；水平方向进行判断，是否和前一个兄弟的值相同，相同则跳过；
2. 迭代：
若ans规模为10；
加入一个不重复元素x0，ans规模：size = 10(不包含x0的部分) + 10(包含x0的部分) = 20；
加入一个重复元素x1，ans规模：size = 10(不包含x0的部分) + 10(包含x0的部分) + 10(包含x0、x1的部分) = 30；