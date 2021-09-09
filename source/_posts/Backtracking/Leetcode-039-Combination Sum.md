---
title: Leetcode-039.Combination Sum
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
[039.Combination Sum](https://leetcode.com/problems/combination-sum/)

### 1、Description
【输入】
给定一个大小为n的整数数组candidates；
candidates性质：元素都是正数；无重复元素；
给定一个整数target；
target性质：正数；
【输出】
求出所有的【组合】：从candidates中拿出一些数字，其和为target。
每一个【组合】用一个一维数组表示；最后返回一个二维数组；
注：返回的答案中，每个【组合】内部不要求有序；即[122] == [221] == [212]，都认为等价；
【要求】
无；

### 2、Example
Input: candidates = [2,3,6,7], target = 7,
Output:
[
  [7],
  [2,2,3]
]

<!-- more -->

### 3、Corner Case
1. target为正数；
2. candidates[i]为正数；
3. candidates中无重复数；
4. n == 0时：而target为正数，return 空二维数组即可；
5. 考虑n >= 1的情况；

## 二、题解

### 0、思考
1. 从上面的例子中可以看出：

2. 对于nums中的任意元素，可以认为其数量是不限的；

3. 【相对顺序】是不重要的，即：[211] == [112] == [121]；也就是求【Combination】，而非【Permutation】；

4. 类似于518.Coin Change 2：518求一共有多少个组合；本题求出每一个组合具体是什么；

### 1、Solution-1：Backtracking
1. candidates不保证是有序的。

2. 若candidates的长度为n。

3. 以candidates[0]为root，对递归树进行DFS，过程如下：

4. 由于每个数字可以使用无限次，所以：遍历完node后，可以继续访问node；

5. 到达每个结点node时：
首先，判断搜索是否还应该继续：若target < 0，则不需要继续搜索了，“此路不通”；返回，回溯到上层。
其次，判断搜索是否顺利结束了：若target == 0，“此路合法”，将其添加到ans中；返回，回溯到上层。
最后，若还需要继续搜索：以DFS的顺序，依次考虑其n个子树；

6. Code(https://leetcode.com/submissions/detail/348891885/)
AC
// 每个结点的“度”为：n；
// 树高h为：(target / min(candidates[i]))
时间复杂度：O(n ^ h)  
空间复杂度：O(h)
```C++
class Solution {
private:
    void backtracking(int startPos, int target, vector<int>& candidates, vector<int>& temp, vector<vector<int>>& ans) {
        if (target < 0) {
            return;
        }
        if (target == 0) {
            ans.push_back(temp);
            return;
        }
        int n = candidates.size();
        for (int i = startPos; i <= n - 1; i++) {
            temp.push_back(candidates[i]);
            backtracking(i, target - candidates[i], candidates, temp, ans);
            temp.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        int n = candidates.size();
        if (n == 0) {
            return {{}};
        }
        vector<vector<int>> ans;
        vector<int> temp;
        backtracking(0, target, candidates, temp, ans);
        return ans;
    }
};
```

7. 剪枝策略：
对candidates进行排序；
当target - candidates[i]为负数的时候，不再进入递归，而在loop的时候控制；
同时，由于candidates是从小到大排序的，当发现第一个不满足的时候，后续都不用判断了；

8. Code(https://leetcode.com/submissions/detail/348956926/)
AC-运行时间从12ms降低为4ms
// 每个结点的“度”相同：都是n；
// 树高h为：(target / min(candidates[i]))
时间复杂度：O(n ^ h)  
空间复杂度：O(h)
```C++
class Solution {
private:
    void backtracking(int startPos, int target, vector<int>& candidates, vector<int>& temp, vector<vector<int>>& ans) {
        if (target == 0) {
            ans.push_back(temp);
            return;
        }
        int n = candidates.size();
        for (int i = startPos; i <= n - 1 && target - candidates[i] >= 0; i++) { // 此处进行控制：若不满足，及时停掉；
            temp.push_back(candidates[i]);
            backtracking(i, target - candidates[i], candidates, temp, ans); // 此处的第二个参数一定为非负数；
            temp.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        int n = candidates.size();
        if (n == 0) {
            return {{}};
        }
        vector<vector<int>> ans;
        vector<int> temp;
        // 1. sort
        sort(candidates.begin(), candidates.end());
        // 2. backtracking
        backtracking(0, target, candidates, temp, ans);
        return ans;
    }
};
```

9. 总结
candidates无重复元素；
允许同一个元素使用多次，即：选择了索引为i的元素，进入递归树的下一层时，依旧允许使用索引为i的元素；
判重：[2,3]和[3,2]认为是重复的；
去重：不走回头路，即：选择了索引为i的元素，进入递归树的下一层时，不允许使用[0, i - 1]的元素了；
剪枝：对candidates进行排序，当遍历到索引为i的元素，发现其不满足要求时，不再考虑[i + 1, n - 1]范围的所有元素；