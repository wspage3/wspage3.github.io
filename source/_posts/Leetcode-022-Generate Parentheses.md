---
title: 22.Generate Parentheses
---

## 一、题意
题目链接: [22.Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
### 1.Description
给定一个整数n，代表有n对括号。这是一对：( )
求出这n对括号所有可能的组合方式，要求组合后是一个有效的括号表达式。
即：匹配的一对括号，左边是(，右边是)
### 2.Example
n = 3
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
### 3.Corner Case
n: 当n为0的时候，直接返回空；

## 二、题解
### Solution-1：Backtracking(Recursive)
1.给定的n，代表有n个open bracket，n个close bracket。

2.对于任意时刻，我们仅可选一个bracket，要么是open的，要么是close的。
只要leftCnt大于0，就可以选择open的。
而选择close的，需要rightCnt大于leftCnt，以保证结果先左后右的合法性。

3.当leftCnt, rightCnt的值都为0时，此时构造出来的string s就是一个合法的答案。将其添加到答案中。
并且返回。（回溯）

4.递归树的某一个子节点计算完成后返回了，其父节点进行考虑，若当前还有其他的儿子，则递归调用其他的儿子。
对于一个节点来说，其左儿子可以认为是“选择open bracket”，其右儿子可以认为是“选择close bracket”

时间复杂度：O() //to do 
空间复杂度：O()

5.Code(https://leetcode.com/submissions/detail/285419056/)











