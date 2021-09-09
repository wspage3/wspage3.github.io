---
title: Leetcode-216.Combination Sum III
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Combination
- DFS
- Backtracking
---

## 一、题意

### 黑板上排列组合，你舍得解开吗~~

### 0、题目链接
[216.Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

### 1、Description
【输入】
给定一个整数target；target性质：正数；
给定一个整数k；k性质：正数；
【输出】
求出所有的【组合】：从[1, 2, 3, 4, 5, 6, 7, 8, 9]中拿出k个数字（每个元素只能用一次），其和为target。
每一个【组合】用一个一维数组表示；最后返回一个二维数组；
注：返回的答案中，每个【组合】内部不要求有序；即[12] == [21]，认为二者等价；
【要求】
无；

### 2、Example
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]

<!-- more -->

### 3、Corner Case
1. target为正数；
2. k为正数；

## 二、题解

### 0、思考
1. 类似于040.Combination Sum II，区别如下：
040中，candidates内可能存在重复数字，所以需要去重；
本题中，candidates固定为[1, 2, 3, 4, 5, 6, 7, 8, 9]，所以不需要sort以及去重；

2. 类似于040，对于candidates中的任意元素，其数量为1；

3. 【相对顺序】是不重要的，即：[21] == [12]；也就是求【Combination】，而非【Permutation】；

### 1、Solution-1：Backtracking
1. Code(https://leetcode.com/submissions/detail/349222583/)
AC
// 每个结点的“度”不同，最大为9
// 树高为：k，k最大值为9
// O(C(9,k)) -> O(9 ^ k)
时间复杂度：O(9 ^ k)  
空间复杂度：O(k)
```C++
class Solution {
private:
    void backtracking(int startNum, int target, vector<int>& temp, vector<vector<int>>& ans, int k) {
        if (target == 0 || temp.size() == k) {
            if (target == 0 && temp.size() == k) {
                ans.push_back(temp);
            }
            return;
        }
        for (int i = startNum; i <= 9 && target - i >= 0; i++) { // 剪枝策略
            temp.push_back(i);
            backtracking(i + 1, target - i, temp, ans, k); // 每个元素只能使用一次
            temp.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<int> temp;
        vector<vector<int>> ans;
        backtracking(1, n, temp, ans, k);
        return ans;
    }
};
```

2. 总结
candidates无重复元素，固定为[1, 9]；
结束条件：k个元素，加起来和为n；
同一个元素只能用一次，即：选择了i后，进入递归树的下一层时，不能继续使用元素i了；
判重1：[2,3]和[3,2]认为是重复的；
去重1：不走回头路，即：选择了i后，进入递归树的下一层时，不允许使用[1, i - 1]的元素了；
剪枝：candidates是有序的，当遍历到元素i，发现其不满足要求时，不再考虑[i + 1, 9]范围的所有元素；
剪枝：两个条件都满足时，路径合法，记录下来；只有一个条件满足时，不再进行搜索，直接返回；