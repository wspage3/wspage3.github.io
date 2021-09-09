---
title: Leetcode-997.Find the Town Judge
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Graph
---

## 一、题意

### 0、题目链接
[997.Find the Town Judge](https://leetcode.com/problems/find-the-town-judge/)

### 1、Description
【输入】
给定一个整数N，代表镇上有N个人，编号从1开始，到N结束；
给定一个大小为len的二维数组trust；
trust[i] = [x, y]，代表居民x信任居民y；
【输出】
传言：镇上有个法官：他不信任任何人；且其它居民都信任他；
判断能否找到法官，如果找到，返回其代表的数字；否则，返回-1；
【要求】
无；

### 2、Example
Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]

<!-- more -->

### 3、Corner Case
1. 1 <= N <= 1000； 
2. 0 <= len <= 10^4

## 二、题解

### 0、思考
1. 将N个人想象成一张图中的N个点；

2. 信任关系代表着一条有向边。若x信任y，则x到y存在一条有向边；

### 1、Solution-1：Graph
1. 遍历trust，统计每个结点的入度、出度。若(入度 - 出度)为N - 1，则该结点是法官；

2. count数组直接记录(入度 - 出度)即可；

3. Code(https://leetcode.com/submissions/detail/345265250/)
AC
时间复杂度：O(N)
空间复杂度：O(N)
```C++
class Solution {
public:
    int findJudge(int N, vector<vector<int>>& trust) {
        int len = trust.size();
        vector<int> count(N + 1, 0); // 入度 - 出度
        for (int i = 0; i <= len - 1; i++) {
            count[trust[i][1]]++;
            count[trust[i][0]]--;
        }
        for (int i = 1; i <= N; i++) {
            if (count[i] == N - 1) {
                return i;
            }
        }
        return -1;
    }
};
```