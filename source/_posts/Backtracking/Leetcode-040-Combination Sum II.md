---
title: Leetcode-040.Combination Sum II
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
[040.Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

### 1、Description
【输入】
给定一个大小为n的整数数组candidates；
candidates性质：元素都是正数；可能有重复元素；
给定一个整数target；
target性质：正数；
【输出】
求出所有的【组合】：从candidates中拿出一些数字（每个元素只能用一次），其和为target。
每一个【组合】用一个一维数组表示；最后返回一个二维数组；
注：返回的答案中，每个【组合】内部不要求有序；即[122] == [221] == [212]，都认为等价；
【要求】
无；

### 2、Example
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]

<!-- more -->

### 3、Corner Case
1. target为正数；
2. candidates[i]为正数；
3. candidates中可能有重复数；
4. n == 0时：而target为正数，return 空二维数组即可；
5. 考虑n >= 1的情况；

## 二、题解

### 0、思考
1. 从上面的例子中可以看出：

2. 对于nums中的任意元素，其数量为1；

3. 【相对顺序】是不重要的，即：[211] == [112] == [121]；也就是求【Combination】，而非【Permutation】；

4. 类似于039.Combination Sum：
039中，candidates无重复数，每个元素可以使用无限次；
本题中，candidates中可能有重复数，每个元素可以使用一次；

### 1、Solution-1：Backtracking
1. candidates不保证是有序的。

2. 若candidates的长度为n。

3. 对candidates进行从小到大排序。
039中，sort是一个可选的动作，为了一种剪枝策略；
本题中，sort是一个必选的动作，为了【去重】；

4. 以candidates[0]为root，对递归树进行DFS，过程如下：

5. 由于每个数字仅可以使用一次，所以：遍历完i后，继续访问i + 1；

6. 由于已经排序了，继续使用039中的剪枝策略；

7. 到达每个结点node时：
其次，判断搜索是否顺利结束了：若target == 0，“此路合法”，将其添加到ans中；返回，回溯到上层。
最后，若还需要继续搜索：以DFS的顺序，依次考虑其若干个子树；

8. 举个例子：
candidates = [1, 1, 2, 5, 6, 7], target = 8
startPos = 0:
i = 0：1-1-6；1-2-5；1-7；
i = 1：continue；去重策略，否则的话，仍然有：1-2-5；1-7；导致ans中冗余；
i = 2：2-6；
i = 3：空
i = 4：空
i = 5：空

9. Code(https://leetcode.com/submissions/detail/349205819/)
AC
// 每个结点的“度”不同，最大为n
// 树高为：n
// Basically, for each num you have two choices, pick it or not.
时间复杂度：O(2 ^ n)  
空间复杂度：O(n)
```C++
class Solution {
private:
    void backtracking(int startPos, int target, vector<int>& candidates, vector<int>& temp, vector<vector<int>>& ans) {
        if (target == 0) {
            ans.push_back(temp);
            return;
        }
        int n = candidates.size();
        for (int i = startPos; i <= n - 1 && target - candidates[i] >= 0; i++) { // 剪枝策略，同039.Combination Sum
            if (i > startPos && candidates[i] == candidates[i - 1]) { // 去重策略
                continue;
            }
            temp.push_back(candidates[i]);
            backtracking(i + 1, target - candidates[i], candidates, temp, ans); // 每个元素只用一次
            temp.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        int n = candidates.size();
        if (n == 0) {
            return {{}};
        }
        
        sort(candidates.begin(), candidates.end());
        vector<int> temp;
        vector<vector<int>> ans;
        backtracking(0, target, candidates, temp, ans);
        return ans;
    }
};
```

10. 总结
candidates有重复元素；
同一个元素只能用一次，即：选择了索引为i的元素，进入递归树的下一层时，不能继续使用索引为i的元素了；
判重1：[2,3]和[3,2]认为是重复的；
去重1：不走回头路，即：选择了索引为i的元素，进入递归树的下一层时，不允许使用[0, i - 1]的元素了；
判重2：假设candidates为[1,2,2]，target为3；则认为[1,2(idx=1)]和[1,2(idx=2)]是重复的；
去重2：在for循环选择不同的路径时，判断当前路径和前一条路径的值是否相等，相等则跳过本路径；
剪枝：对candidates进行排序，当遍历到索引为i的元素，发现其不满足要求时，不再考虑[i + 1, n - 1]范围的所有元素；
