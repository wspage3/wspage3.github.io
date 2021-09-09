---
title: Leetcode-901.Online Stock Span
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Monotonous Stack
- Stack
- Design
---

## 一、题意

### 0、题目链接
[901.Online Stock Span](https://leetcode.com/problems/online-stock-span/)

### 1、Description
【输入】
无；
【输出】
设计一个类StockSpanner，支持如下操作：
int next(int price)：
price代表当天的股价；
求其从今天开始，往前数，看【连续】几天的股价【小于等于】今天的股价price；
注：包含今天；
【要求】
无；

### 2、Example
Input: 
["StockSpanner","next","next","next","next","next","next","next"],
[ [],[100],[80],[60],[70],[60],[75],[85] ]
Output: 
[null, 1, 1, 1, 2, 1, 4, 6]

<!-- more -->

### 3、Corner Case
1. 1 <= price <= 10^5 

## 二、题解

### 0、思考
1. 对于任意元素x来说，只关心其左侧的元素，即可求出x对应的答案。

2. 对于x来说，求：【左侧】【连续的】【小于等于我】的元素个数。

3. 类似于739.Daily Temperatures，使用【单调递减栈】来解决。

### 1、Solution-1：Monotonous Stack
1. 举个例子：T = [100, 80, 60, 70, 60, 75, 85]，n = 7；
i = 0：[(100, 1) ；ans[100] = 1 + 0 = 1；
i = 1：[(100, 1)(80, 1) ；ans[80] = 1 + 0 = 1；
i = 2：[(100, 1)(80, 1)(60, 1) ；ans[60] = 1 + 0 = 1；
i = 3：[(100, 1)(80, 1)(70, 2) ；ans[70] = 1 + ans[60] = 2；
i = 4：[(100, 1)(80, 1)(70, 2)(60, 1) ；ans[60] = 1 + 0 = 1；
i = 5：[(100, 1)(80, 1)(75, 4) ；ans[75] = 1 + ans[60] + ans[70] = 4；
i = 6：[(100, 1)(85, 6) ；ans[85] = 1 + ans[75] + ans[80] = 6；
ans = [1, 1, 1, 2, 1, 4, 6]

2. 观察：
从栈底，到栈顶，元素严格递减；
一旦x比栈顶元素【大于等于】，出栈若干次；
如果某个元素在栈中，代表：我还有存在的意义，即对后面的元素可能还有影响；
如果某个元素出栈了，代表：我没必要存在了，我的那部分信息已经被我后面的某个元素记录下来了；

6. Code(https://leetcode.com/submissions/detail/348073360/)
AC
时间复杂度：O(1) // Amortized O(1)，Worst O(n)
空间复杂度：O(1)
```C++
class StockSpanner {
private:
    stack<pair<int, int>> s;
public:
    StockSpanner() {
        
    }
    
    int next(int price) {
        int ans = 1;
        while (!s.empty() && price >= s.top().first) {
            ans += s.top().second;
            s.pop();
        }
        s.push({price, ans});
        return ans;
    }
};
```