---
title: Leetcode-077.Combinations
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
[077.Combinations](https://leetcode.com/problems/combinations/)

### 1、Description
【输入】
给定一个整数n；
给定一个整数k；
【输出】
求出所有的【组合】：从[1, 2, ..., n]中拿出k个数字（每个元素只能用一次）。
每一个【组合】用一个一维数组表示；最后返回一个二维数组；
注：返回的答案中，每个【组合】内部不要求有序；即[12] == [21]，认为二者等价；
【要求】
无；

### 2、Example
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

<!-- more -->

### 3、Corner Case
1. k == 0时，return空二维数组；
2. n == 0时，return空二维数组；
3. k > n时，即使把[1, n]中所有元素考虑了，也凑不到k个元素。return空二维数组；
4. 考虑k、n均为正数，且k <= n的情况；

## 二、题解

### 0、思考
1. 类似于216.Combination Sum III，异同点如下：
216中，candidates固定为[1, 2, 3, 4, 5, 6, 7, 8, 9]；每个元素仅使用一次；凑k个；和为target；
本题中，candidates为[1, 2, ... , n]；每个元素仅使用一次；凑k个；

2. 【相对顺序】是不重要的，即：[21] == [12]；也就是求【Combination】，而非【Permutation】；

### 1、Solution-1：Backtracking
1. Code(https://leetcode.com/submissions/detail/349296871/)
AC
// 每个结点的“度”不同，最大为n
// 树高为：k
// O(C(n,k)) -> O(n ^ k)
时间复杂度：O(n ^ k)  
空间复杂度：O(k)
```C++
class Solution {
private:
    void backtracking(int startNum, vector<int>& temp, vector<vector<int>>& ans, int k, int n) {
        if (temp.size() == k) {
            ans.push_back(temp);
            return;
        }
        for (int i = startNum; i <= n; i++) {
            temp.push_back(i);
            backtracking(i + 1, temp, ans, k, n);
            temp.pop_back();
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        if (n <= 0 || k <= 0 || k > n) {
            return {{}};
        }
        vector<int> temp;
        vector<vector<int>> ans;
        backtracking(1, temp, ans, k, n);
        return ans;
    }
};
```

2. 剪枝策略：考虑[1, 2, 3, 4, 5, 6]，k = 3
对于root结点来说：此时temp.size() == 0，i从1开始，到4结束即可。因为若temp以5、6开头，最终一定凑不够k个元素。
通过观察总结，得出下面的结论：
若此时temp中有x个元素，代表还需要k - x个元素。i从startNum开始，到n - (k - x) + 1即可。
此时：1 <= k - x <= k
则：n + 1 - (k - x)在范围[n + 1 - k, n]内；
k最大为n：[1, n]
k最小为1：[n, n]
可以看出，不管哪种情况，这个值都是一个有效值。

3. Code(https://leetcode.com/submissions/detail/349306225/)
AC，运行时间从36ms降低到8ms
// 每个结点的“度”不同，最大为n
// 树高为：k
// O(C(n,k)) -> O(n ^ k)
时间复杂度：O(n ^ k)  
空间复杂度：O(k)
```C++
class Solution {
private:
    void backtracking(int startNum, vector<int>& temp, vector<vector<int>>& ans, int k, int n) {
        if (temp.size() == k) {
            ans.push_back(temp);
            return;
        }
        for (int i = startNum; i <= n - (k - temp.size()) + 1; i++) {
            temp.push_back(i);
            backtracking(i + 1, temp, ans, k, n);
            temp.pop_back();
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        if (n <= 0 || k <= 0 || k > n) {
            return {{}};
        }
        vector<int> temp;
        vector<vector<int>> ans;
        backtracking(1, temp, ans, k, n);
        return ans;
    }
};
```

4. 总结
candidates无重复元素，固定为[1, n]；
结束条件：k个元素；
同一个元素只能用一次，即：选择了i后，进入递归树的下一层时，不能继续使用元素i了；
判重1：[2,3]和[3,2]认为是重复的；
去重1：不走回头路，即：选择了i后，进入递归树的下一层时，不允许使用[1, i - 1]的元素了；
剪枝：need记录此时还需要几个元素，i从start开始，到n - need + 1即可，不必每次遍历到n为止；