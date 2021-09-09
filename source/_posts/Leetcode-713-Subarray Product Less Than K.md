---
title: Leetcode713-Subarray Product Less Than K
---

## 一、题意
题目链接: [Leetcode713-Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)
### 1.Description
给定一个长度为n的正整数数组nums；给定一个整数k；
求出满足条件的连续子数组的个数cnt：这个子数组的product小于给定的k
### 2.Example
Input: nums = [10, 5, 2, 6], k = 100
Output: 8
Explanation: 
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
### 3.Corner Case
0 < nums.length <= 50000.
0 < nums[i] < 1000.
0 <= k < 10^6.
需要对k是否为0判断：若k为0，根据题目正整数数组的描述，直接返回0即可

## 二、题解
【连续子数组的乘积】相关的题目。
### Solution-1：Brute Force
1.i从[0, n - 1]遍历。
每次固定i，然后j从[i, n - 1]遍历：找出以i开头所有的连续子数组。其中累乘product，若小于k，则计数器加一。
结果发现Runtime Error：累乘的数据太大了，以至于用long long也会溢出。
2.一个显而易见有问题的地方在于：
给定的k的范围为[0, 1e6]，而我们固定i，试图找出以i开头的所有连续子数组的时候，为什么还会导致累乘的积溢出long long呢？
显然是因为：当发现累乘的积大于k的时候，没有跳出循环，直接考虑i + 1。（由于nums数组中的元素都是正数）
修改后发现结果是：TLE超时。

时间复杂度：O(n ^ 2)
空间复杂度：O(1)

2.Code(https://leetcode.com/submissions/detail/283569919/)

### Solution-2：Sliding Window
考虑数组[a, b, c, d, e, f, g, h, i]

1.首先我们考虑以a开头的子树组：a,ab,abc,abcd,abcde......
若a本身大于等于k，则以a开头的所有子数组都不符合条件，跳过到b。
若a本身不大于k，则可以继续考察ab,abc,abcd,abcde......，某个时刻发现abcdefg不符合条件了：
但我们知道：abcdef是ok的，说明：abcdef内的任意连续子数组都是满足的。
Solution1的做法是break后考察以b开头的。这样的话，就会有重复计算了，不能充分利用上述已知信息。

2.退一步，假设当前位置在"f"，我们已经知道了abcde是合法的，同时abcdef也是合法的。
考虑：把当前的"f"添加到abcde中，新增了一个元素，对应新增了多少个满足条件的子数组呢？
结果是：f, ef, def, cdef, bcdef, abcdef共6个，也就是index(f) - index(a) + 1。
同理我们知道序列abcde对应的答案为：1 + 2 + 3 + 4 + 5 = 15
那么abcdef对应的答案就是：15 + 6 = 21

3.进一步，当前位置在"g"，当发现abcdefg不合法的时候，说明此时答案无法在21的基础上直接加上7。
这时，我们找到以g为结尾的满足条件的一个最大子数组，假设是序列cdefg。即：abcdefg,bcdefg均不合法；而cdefg合法。
此时就可以将g对应的cnt添加到已有的21中了：g, fg, efg, defg, cdefg。
而且这些包含g的子数组不可能和之前的21个子数组有相同的。
同时我们跳过的a和b，他们所有满足的情况都已经被考虑过了。也不会造成遗漏。
然后我们再在这个cdefg的基础上，考察下一个位置"h"......直到所有位置都被考察过了，最后的答案就是本题目的答案。

4.根据以上思路，进行实现：
用一个start指针指向当前window的开始，初始化为0。
每次考察数组nums[i]，i的范围[0, n - 1]，依次将nums[i]添加到window中，并进行判断。
如果此时合法，则考察[i + 1]。并累加：i - start + 1。
如果此时非法，则将window向后滑动，找到start的位置，使得区间[start, i](start <= i>)合法。并累加：i - start + 1。
如果此时发现start > i，则说明start == i + 1，且i位置不存在满足条件的子数组，累加0即可。然后考察i + 1

时间复杂度：O(n)
空间复杂度：O(1)

3.Code(https://leetcode.com/submissions/detail/283582840/)



