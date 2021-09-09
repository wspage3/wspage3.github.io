---
title: Leetcode-155.Min Stack
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Design
- Stack
---

## 一、题意

### 0、题目链接
[155.Min Stack](https://leetcode.com/problems/min-stack/)

### 1、Description
【输入】
无；
【输出】
设计一个数据结构"最小栈"，具有以下操作：
1.push(x) -- Push element x onto stack.
2.pop() -- Removes the element on top of the stack.
3.top() -- Get the top element.
4.getMin() -- Retrieve the minimum element in the stack.
【要求】
getMin()的时间复杂度为O(1)；

### 2、Example
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[], [-2], [0], [-3], [], [], [], []]

Output
[null, null, null, null, -3, null, 0, -2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2

<!-- more -->

### 3、Corner Case
1. 题目数据保证：pop()、top()、getMin()这三个操作调用的时候，栈一定为非空；

## 二、题解

### 0、思考
1. 其中核心功能是getMin()，因为如果仅考虑前三个功能的话，相当于普通的stack。

2. 第一想法：维护一个stack s和一个最小值m即可。当有更小的值入栈后，更新m即可。
但这样有一个问题：如果当前pop()掉的就是最小值m，再次getMin()，那怎么办？
因为没有保存第二小的值，所以在pop()掉m后，此时栈中最小的值就无法确定了。

### 1、Solution-1：Using Two Stacks
1. 用两个stack：一个保存数据（s），一个保存最小值（minStack）。

2. 初始化：当栈为空时，新加入一个元素x。将x压入s中，将x压入minStack中。

3. push(y)：
当新加入一个元素y，y > x：则将y压入s中。
当新加入一个元素y，y <= x：则将y压入s中，同时将y压入minStack中。 

4. getMin()：
可以发现，任意时刻（栈s非空）栈中的最小值m，就是minStack.top()。

5. pop()：
当pop一个元素z的时候，若z > m：则z的离开不会影响到栈中最小值m的变化。
当pop一个元素z的时候，若z == m：则z的离开会影响到栈中最小值m的变化，此时将minStack中栈顶也pop()即可。

6. top()：
返回s.top()即可。

7. Code(https://leetcode.com/submissions/detail/285879960/)
AC
时间复杂度：O(1) // 上述4个操作的时间复杂度均为O(1)
空间复杂度：O(n)
```C++
class MinStack {
private:
    stack<int> s;
    stack<int> minStack;
public:
    MinStack() {
        
    }
    
    void push(int x) {
        s.push(x);
        if (minStack.empty() || x <= minStack.top()) {
            minStack.push(x); 
        }
    }
    
    void pop() {
        if (s.top() == minStack.top()) {
            minStack.pop();
        }
        s.pop();
    }
    
    int top() {
        return s.top();
    }
    
    int getMin() {
        return minStack.top();
    }
};
```

### 2、Solution-2：Using One Stack
1. 采用类似Solution-1中的方法，将所有的最小值保存下来。当最小值“离开”的时候，使用第二最小值来“替补”；

2. 但在本方法中：
用一个整型变量minValue记录当前的最小值；
不再使用minStack来存“替补们”，而是将“替补们”直接存在数据栈s中。

3. 初始化：minValue = INT_MAX;

4. push(y)：
当栈为空时，新加入一个元素y。一定有：y <= minValue。
当y <= minValue时：保存上一个的最小值（将minValue压入s）；更新当前的最小值（minValue = y）；然后再y压入s；
当y > minValue时：将y压入s即可。

5. getMin()：
任意时刻（栈s非空），minValue都是栈s中的最小值。

6. pop()：
当pop一个元素z的时候，若z > minValue：则z的离开不会影响到栈中最小值minValue的变化。
当pop一个元素z的时候，若z == minValue：则z的离开会影响到栈中最小值m的变化。此时将z从s中pop后，s.top()就是上一个最小值，更新minValue即可；

7. top()：
返回s.top()即可。

8. Code(https://leetcode.com/submissions/detail/285881659/)
AC
时间复杂度：O(1) // 上述4个操作的时间复杂度均为O(1)
空间复杂度：O(n)
```C++
class MinStack {
private:
    stack<int> s;
    int minValue;
public:
    MinStack() {
        minValue = INT_MAX;
    }
    
    void push(int x) {
        if (x <= minValue) {
            s.push(minValue);
            minValue = x;
        }
        s.push(x);
    }
    
    void pop() {
        if (s.top() == minValue) {
            s.pop();
            minValue = s.top();
        }
        s.pop();
    }
    
    int top() {
        return s.top();
    }
    
    int getMin() {
        return minValue;
    }
};
```

### 3、Solution-3：Using One Stack
1. 现在我们将当前元素x和minValue的差值存入栈s中。

2. 初始化：
minValue = INT_MAX;
当栈为空时，新加入一个元素y。将0压入s中，更新minValue = y；

3. push(y)：
将y - minValue压入s中；若y < minValue，更新minValue；

4. getMin()：
任意时刻（栈s非空），minValue都是栈s中的最小值。

5. pop()：
当s栈顶元素小于0的时候，说明：这个元素是当前栈中最小的值minValue。更新minValue = minValue - s.top()；然后再从s中把其pop。
当s栈顶元素不小于0的时候，说明，这个元素是一个差值，直接pop即可。

6. top()：
由于s中存的已经不是原始的数据了，而是一个差值。
当s栈顶元素小于0的时候，说明：这个元素是当前栈中最小的值minValue。return minValue即可。
当s栈顶元素不小于0的时候，说明，这个元素是一个差值，其值 = s.top() + minValue。

7. 缺点1：差值可能会导致整数溢出，所以用数据用long long。
缺点2：由于放入栈中的值是一个差值，而非原本的数据本身。

8. Code(https://leetcode.com/submissions/detail/285885660/)
AC
时间复杂度：O(1) // 上述4个操作的时间复杂度均为O(1)
空间复杂度：O(n)
```C++
class MinStack {
    stack<long long> s;
    long long minValue;
public:
    MinStack() {
        minValue = INT_MAX;
    }
    
    void push(int x) {
        if (s.empty()) {
            minValue = x;
            s.push(0);
        } else {
            s.push(x - minValue);
            if (x < minValue) {
                minValue = x;
            }
        }
    }
    
    void pop() {
        if (s.top() < 0) {
            minValue = minValue - s.top();
            s.pop();
        } else {
            s.pop();
        }
        
    }
    
    int top() {
        if (s.top() < 0) {
            return minValue;
        } else {
            return s.top() + minValue;
        }
    }
    
    int getMin() {
        return minValue;
    }
};
```