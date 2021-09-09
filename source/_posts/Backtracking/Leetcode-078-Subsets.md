---
title: Leetcode-078.Subsets
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Backtracking
- DFS
- Subset
- Bit Manipulation
---

## 一、题意

### 0、题目链接
[078.Subsets](https://leetcode.com/problems/subsets/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；
nums性质：无重复元素；
【输出】
求出nums的所有【子集】（包括空集和nums本身）；
每一个【子集】用一个一维数组表示；最后返回一个二维数组；
【要求】
无；

### 2、Example
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

<!-- more -->

### 3、Corner Case
1. n == 0时，返回空二维数组；
2. 考虑n为正数的情况；

## 二、题解

### 0、思考
1. 对于n个元素的nums来说，其子集个数为：2 ^ n；

2. 判断重复：
[1,2,2]
[[],[1],[2(idx=1)],[1,2(idx=1)],[2(idx=2)],[1,2(idx=2)],[2,2],[1,2,2]]
本题要求这种重复情况也都输出；

### 1、Solution-1：Backtracking
1. 考虑递归树，递归树上的每一个结点，都是一个答案；

2. Code(https://leetcode.com/submissions/detail/423550294/)
AC
时间复杂度：O(n * num) // num = 2 ^ n；一共2^n个状态，每种状态需要O(n)的时间来构造子集。
空间复杂度：O(n)
```C++
class Solution {
private:
    void backtracking(int start, vector<int>& nums, vector<int>& sub, vector<vector<int>>& ans) {
        int n = nums.size();
        ans.push_back(sub);
        for (int i = start; i <= n - 1; i++) {
            sub.push_back(nums[i]);
            backtracking(i + 1, nums, sub, ans);
            sub.pop_back();
        }
    }
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> sub;
        vector<vector<int>> ans;
        backtracking(0, nums, sub, ans);
        return ans;
    }
};
```

### 2、Solution-2：Iterative
1. 举例：
nums = [1, 2, 3]
初始化：ans = [[]];
i = 0：ans = [[], [1]]
i = 1：ans = [[], [1], [2], [1, 2]]
i = 2：ans = [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]

2. Code(https://leetcode.com/submissions/detail/351147997/)
AC
时间复杂度：O(n * num) // num = 2 ^ n；一共2^n个状态，每种状态需要O(n)的时间来构造子集。
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return {{}};
        }
        
        vector<vector<int>> ans = {{}};
        for (int i = 0; i <= n - 1; i++) {
            int cur = nums[i];
            int size = ans.size();
            for (int j = 0; j <= size - 1; j++) {
                ans.push_back(ans[j]);
                ans.back().push_back(cur);
            }
        }
        
        return ans;
    }
};
```

### 3、Solution-3：Bit Manipulation
1. 观察：
nums = [1, 2, 3]
i = 0：0 0 0 -> [     ]
i = 1：0 0 1 -> [    1] 右移0位
i = 2：0 1 0 -> [  2  ] 右移1位  
i = 3：0 1 1 -> [  2 1] 右移0、1位 
i = 4：1 0 0 -> [3    ] 右移2位
i = 5：1 0 1 -> [3   1] 右移0、2位
i = 6：1 1 0 -> [3 2  ] 右移1、2位
i = 7：1 1 1 -> [3 2 1] 右移0、1、2位

2. 举例：
nums = [1, 3, 5]
初始化：ans = [[], [], [], [], [], [], [], []];
i = 0：
i = 1：满足条件：j = 0；      ans = [[], [1], [], [], [], [], [], []];
i = 2：满足条件：j = 1；      ans = [[], [1], [3], [], [], [], [], []];
i = 3：满足条件：j = 0、1；   ans = [[], [1], [3], [1, 3], [], [], [], []];
i = 4：满足条件：j = 2；      ans = [[], [1], [3], [1, 3], [5], [], [], []];
i = 5：满足条件：j = 0、2；   ans = [[], [1], [3], [1, 3], [5], [1, 5], [], []];
i = 6：满足条件：j = 1、2；   ans = [[], [1], [3], [1, 3], [5], [1, 5], [3, 5], []];
i = 7：满足条件：j = 0、1、2；ans = [[], [1], [3], [1, 3], [5], [1, 5], [3, 5], [1, 3, 5]];

3. 5的二进制：[0101]；5 & 1 = 1；
2的二进制：[0010]；2 & 1 = 0；
x & 1可以看x是否为奇数；

4. Code(https://leetcode.com/submissions/detail/351168091/)
AC
时间复杂度：O(n * num) // num = 2 ^ n；一共2^n个状态，每种状态需要O(n)的时间来构造子集。
空间复杂度：O(1)
```C++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return {{}};
        }
        
        int size = 1 << n; // 2 ^ n
        vector<vector<int>> ans(size);
        for (int i = 0; i <= size - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if ((i >> j) & 1) {
                    int cur = nums[j];
                    ans[i].push_back(cur);
                }
            }
        }
        
        return ans;
    }
};
```

### 4、总结
1. 回溯：递归树的每个结点都是一个答案；[1,2]和[2,1]是等价的，用“不走回头路”实现；
2. 迭代：每加入一个元素，ans规模翻一倍；
3. 位运算