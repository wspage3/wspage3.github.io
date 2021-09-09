---
title: 394.Decode String
---

## 一、题意
题目链接: [394.Decode String](https://leetcode.com/problems/decode-string/)
### 1.Description
【输入】
给定一个长度为s的字符串。这个字符串可以理解为“加密串”。
【输出】
找出该字符串对应的“解密串”。
规则：k[str]，代表str出现k次。此处保证k一定是正整数。

### 2.Example
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".

### 3.Corner Case
1. 保证方括号都是配对，且合法的。
2. 保证出现的数字都是正整数。
3. “解密串”中不包含字母。所以说“加密串”中出现的数字一定是k，代表次数。
4. “加密串”中的“[]”内部不包含数字。

## 二、题解
### Solution-1：DFS
1.从前到后考虑字符串s的每一个位置：[0, n - 1]
开始调用：ans = dfs(0, s);

2.若位置i遇到'['，则说明遇到一个子问题，进行递归调用:
substr = dfs(i + 1, s);
假设这个子问题的答案已经解出来了，答案为："abcabc"。

3.任意一个子问题要想解出来答案，说明当前位置j出现了']'。
此时将“计算拼接”出来的子答案返回给上层函数。

4.回到步骤2，发现：
位置i出现了'['，寻找子问题的答案。
位置j出现了']'，返回子问题的答案为"abcabc"。
说明区间[i, j]上的所有字符都被考虑过了。其value就是"abdabc"。
此时我们将i位置之前的num找出来，将其拼接到一起。
然后继续考虑位置(j + 1)......
注意：由于父亲需要知道子问题返回的时候，走到了哪个位置，所以dfs函数的第一个参数用一个int引用。

5.继续遍历，知道i > n - 1为止。

时间复杂度：O(n)
空间复杂度：O(n) //递归开销

6.Code(https://leetcode.com/submissions/detail/286626640/)
```C++
class Solution {
private:
    string dfs(int& i, string s) {
        string word = "";
        int num = 0;
        while (i < s.size()) {
            char c = s[i];
            if (c == '[') {
                string next = dfs(++i, s);
                for (int i = 0; i <= num - 1; i++) {
                    word += next;
                }
                num = 0;
            } else if (c >= '0' && c <= '9') {
                num = num * 10 + c - '0';
            } else if (c == ']') {
                return word;
            } else {
                word += c;
            }
            i++;
        }
        return word;
    }
public:
    string decodeString(string s) {
        int i = 0;
        return dfs(i, s);
    }
};
```

### Solution-2：Stack
1.若没有方括号嵌套，while循环从前到后一个一个的找出子答案，拼接起来就行。
遇到一个'['，则继续向后找，直到找到一个']'，说明这两个是匹配的括号。
将前面的这个范围内的字符循环num次即可，拼接到答案ans上。num清零。考虑下一个位置。

2.当存在括号嵌套的时候，当找到一个'['，后面可以继续跟着一个'['，
此时计算需要由内往外，找出最内层的匹配的一对方括号，将其答案计算出来，返回给上一层计算......

3.可以观察到，上面先遍历到的'['，反而是最后计算，这种特性十分符合栈的“先入后出”。我们用栈来模拟。
使用两个辅助栈：
int类型的sNum: 当出现'['时，将此时num入栈。num清零。
string类型的sString: 当出现'['时，将此时已有的答案ans入栈。对于第一次入栈来说，一定是""。ans清空。
这样我们就把各个状态存起来了。

4.当出现一个']'的时候，说明某个子问题计算出来了。
此时：sNum栈顶的元素就是其出现的次数。
sString栈顶的元素就是其之前已有的答案。
ans就是其出现一次的情况。
将答案字符串更新为：ans = sString.top() + sNum.top() * ans；
然后从两个栈中各自pop出一个元素。

5.举个例子：3[a2[c]]    n = 8
i = 0:  '3'  ans = ""  num = 3 
i = 1:  '['  push("")  push(3)  ans = ""  num = 0
i = 2:  'a'  ans = "a"
i = 3:  '2'  num = 2
i = 4:  '['  push("a")  push(2)  ans = ""  num = 0
i = 5:  'c'  ans = "c"
i = 6:  ']'  ans = "a" + 2 * "c" = "acc"  pop("a")  pop(2)
i = 7:  ']'  ans = "" + 3 * "acc" = "accaccacc"  pop("")  pop(3)

时间复杂度：O(n)
空间复杂度：O(n)

6.Code(https://leetcode.com/submissions/detail/286636665/)
```C++
class Solution {
public:
    string decodeString(string s) {
        int n = s.size();
        int i = 0;
        int num = 0;
        string ans = "";
        stack<int> sNum;
        stack<string> sString;
        while (i <= n - 1) {
            char c = s[i];
            if (isdigit(c)) {
                num = num * 10 + c - '0';
            } else if (isalpha(c)) {
                ans += c;
            } else if (c == '[') {
                sNum.push(num);
                sString.push(ans);
                num = 0;
                ans = "";
            } else if (c == ']') {
                string s = "";
                for (int i = 0; i <= sNum.top() - 1; i++) {
                    s += ans;
                }
                ans = sString.top() + s;
                sNum.pop();
                sString.pop();
            }
            i++;
        }
        return ans;
    }
};
```