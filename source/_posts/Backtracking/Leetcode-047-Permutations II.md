---
title: Leetcode-047.Permutations II
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Permutation
- DFS
- Backtracking
---

## 一、题意

### 黑板上排列组合，你舍得解开吗~~

### 0、题目链接
[047.Permutations II](https://leetcode.com/problems/permutations-ii/)

### 1、Description
【输入】
给定一个长度为n的整数数组nums；
nums性质：元素可能重复；
【输出】
求出所有的【排列】：从nums中拿出全部数字（每个元素用一次，不重复，不遗漏）。
每一个【排列】用一个一维数组表示；最后返回一个二维数组；
注：【排列】考虑内部的次序，即[12] != [21]；
【要求】
无；

### 2、Example
Input: [3,1,2,1]
Output:
[
    [1,1,2,3],
    [1,1,3,2],
    [1,2,1,3],
    [1,2,3,1],
    [1,3,1,2],
    [1,3,2,1],
    [2,1,1,3],
    [2,1,3,1],
    [2,3,1,1],
    [3,1,1,2],
    [3,1,2,1],
    [3,2,1,1]
]

<!-- more -->

### 3、Corner Case
1. n == 0时，return空二维数组；
2. 考虑n为正数的情况；

## 二、题解

### 0、思考
1. 【相对顺序】是重要的，即：[21] != [12]；也就是求【Permutation】，而非【Combination】；

2. 046.Permutations中：从nums中拿出全部数字；求【排列】；nums无重复元素；
本题中：从nums中拿出全部数字；求【排列】；nums可能有重复元素；

### 1、Solution-1：Backtracking
1. 由于重复元素的存在，需要考虑去重。

2. 去重思路：
对nums进行排序；
对于结点node来说，假设考虑完其第一个儿子x后，考虑其第二个儿子y：若x->val == y->val，则跳过y；

2. Code(https://leetcode.com/submissions/detail/350166592/)
AC
// 每个结点的“度”不同，最大为n
// 树高为：n
时间复杂度：O(n * n!) // 需要数学证明，参考官方题解。
空间复杂度：O(n)
```C++
class Solution {
private:
    void backtracking(vector<int>& nums, vector<int>& temp, vector<vector<int>>& ans, vector<bool>& visited) {
        int n = nums.size();
        if (temp.size() == n) {
            ans.push_back(temp);
            return;
        }
        
        for (int i = 0; i <= n - 1; i++) {
            // if的第二个条件：去重策略
            if (visited[i] == true || (i >= 1 && nums[i] == nums[i - 1] && visited[i - 1] == false)) {
                continue;
            }
            temp.push_back(nums[i]);
            visited[i] = true;
            backtracking(nums, temp, ans, visited);
            temp.pop_back();
            visited[i] = false;
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return {{}};
        }
        
        sort(nums.begin(), nums.end());
        
        vector<int> temp;
        vector<vector<int>> ans;
        vector<bool> visited(n, false);
        backtracking(nums, temp, ans, visited);
        return ans;
    }
};
```

3. 总结
nums有重复元素；
结束条件：n个元素；
同一个元素只能用一次，即：选择了i后，进入递归树的下一层时，不能继续使用元素i了；
判重1：[2,3]和[3,2]认为是不重复的，都需要考虑到；
实现1：走回头路，即：选择了i后，进入递归树的下一层时，仍可考虑[0, i - 1]的元素；
判重2：当nums为[2,2,3]时：[2(idx=0),2(idx=1),3]和[2(idx=1),2(idx=0),3]认为是重复的，需要去重；
去重2：在递归树中，若当前结点的值和左边兄弟结点的值相同，则跳过当前分支；
注意：水平方向上，两个兄弟结点若相等，只取左边那个分支，其余的舍弃；竖直方向上，允许父子结点的值相同；