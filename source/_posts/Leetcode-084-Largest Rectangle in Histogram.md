---
title: Leetcode84-Largest Rectangle in Histogram
---

## 一、题意
题目链接: [Leetcode84-Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
### 1.Description
给定一个长度为n的非负整数数组heights；heights[i]代表第个矩形的高度；矩形宽度都是1；
求出这个图中最大的矩形面积。
### 2.Example
 input = [2,1,5,6,2,3]
output = 10
### 3.Corner Case
n：当n为0和1的时候，直接返回结果；
heights[i]：题目保证非负

## 二、题解
### Solution-1：Brute Force
1.给定两个不同的柱子i, j，则area[i, j] = max(heights[i], heights[j], (j - i + 1) * minHeightBetweenIAndJ);
其中minHeightBetweenIAndJ是[i, j]中最短的高度。此过程需要O(n)的时间。
2.所以可以考察任意两个不同的i, j。i：[0, n - 2] j：[i + 1, n - 1]

时间复杂度：O(n ^ 3)
空间复杂度：O(1)：
结果发现：此方法TLE，说明需要进一步优化。

3.Code(https://leetcode.com/submissions/detail/283879193/)

### Solution-2：Brute Force
1.由于我们遍历i, j是有序情况下的。在计算出[i, j]之间的结果后，立马考虑[i, j + 1]。
此时发现还要遍历n次找到[i, j + 1]中的最低高度，而[i, j]中的最低高度在之前已经计算过了。
同时minHeight[i, j + 1] = min(minHeight[i, j], heights[j + 1]);
所以保留这个结果，以O(1)的time就可以得到minHeight[i, j + 1]

时间复杂度：O(n ^ 2)
空间复杂度：O(1)：
结果发现：此方法TLE，说明需要进一步优化。

2.Code(https://leetcode.com/submissions/detail/283882070/)

### Solution-3：Brute Force
1.换一种不同于Solution1和Solution2的思路：
在上面的枚举方法中，我们是枚举矩形能出现的所有区间[i, j]，固定了区间[i, j]，然后在这个区间中寻找一个最短的长度。
换一种思路：考虑每个位置i，找出以当前heights[i]为高度的，最大矩形的面积res[i]。则答案就是res数组的最大值。

2.对于当前heights[i]来说，需要找到在[0, i - 1]中从后往前第一个比heights[i]小的值leftI。
同理，需要找到[i + 1, n - 1]中从前往后第一个比heights[i]小的值rightI。
则res[i] = heights[i] * (rightI - leftI - 1)

时间复杂度：O(n ^ 2)
空间复杂度：O(n)：
结果发现：此方法TLE，说明需要进一步优化。

3.Code(https://leetcode.com/submissions/detail/283922360/)

### Solution-4：Monotonous Stack
1.回忆Leetcode739-Daily Temperatures：该问题，我们用Brute Force方法：对于nums[i]，要做的就是找到它后面第一个比它大的元素。
同时我们用单调栈的方法使得上述的O(n ^ 2)优化为O(n)。

2.再回过头来看本题目，对于heights[i]，实质上也是寻找它左右两边第一个比它小的元素。
如果我们能在O(n)的时间内，找到所有heights[i]对应的leftI和rightI。那么上面Solution3的时间复杂度就能降低到O(n)。

3.Leetcode739-Daily Temperatures的单调栈解法是：
维护一个单调栈，且栈从顶到底是递增的；
当一个元素比栈中某个元素大的时候，将栈中元素抛出k个，直到当前元素不大于栈顶元素。
这样的话，对于当前元素，我们可以找到它左边第一个比它小的元素。
对于栈中元素，当其出栈时，当前元素就是其右边第一个比它大的元素。（Leetcode739思路就是依据本条）

4.类似的，维护一个单调栈，且栈从顶到底是递减的；
栈中元素指的是对应的数组下标。
当我们在位置cur的时候，heights[cur]比栈中某个元素小的时候，将栈中元素抛出k个，直到当前元素不大于栈顶元素。
对于抛出去的k个元素：
其rightI就是cur。
其leftI就是该元素在栈中的下面一个元素，（因为栈任意时刻都是自顶向下递减），如果栈为空了，说明刚刚被抛出去的这个元素在左边没有比它小的，记为-1。
然后不断更新，直至遍历完依次heights数组。
此时栈中可能还有元素：则它们都是因为自身右边没有比自身小的元素。将其rightI记录为n。
这是因为，在[a, b]区间内，不包含a，b本身，一共有(b - a - 1)个元素。当其左边没有，记录为-1；当其右边没有，记录为n；方便计算。

时间复杂度：O(n)
空间复杂度：O(n)：

3.Code(https://leetcode.com/submissions/detail/283927850/)


























