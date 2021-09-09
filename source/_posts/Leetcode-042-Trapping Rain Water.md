---
title: Leetcode42-Trapping Rain Water
---

## 一、题意
题目链接: [Leetcode42-Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
### 1.Description
给定一个长度为n的非负整数数组height；height[i]代表第个石头的高度；石头宽度都是1；
求出这个图中所有石头可以存储的水的体积。
### 2.Example
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
### 3.Corner Case
n：当n为0/1/2的时候，直接返回0；
heights[i]：题目保证非负

## 二、题解
### 分析
要想可以存储水，则n的数目至少为2。
例如[2,1,3]此时，由于中间高度为1，两边都比它高，所以可以存储水，水量为min(2, 3) - 1 = 1。
所以，我们对[0, n - 1]的每个位置进行遍历，找出每个位置可以存储的水量resI，则答案就是所有resI的和。
对于位置i来说，需要找到[0, i - 1]最高的高度a；以及[i + 1, n - 1]最高的高度b；
且min(a, b)大于height[i]的时候，位置i才可以存储水。
所以问题转化为：求给定一个元素i，它左边最大的值，以及它右边最大的值。

### Solution-1：Brute Force
1.i从[0, n - 1]遍历。
对于每个i，leftMax和rightMax记录其左右两侧最大的值，则res[i] = min(leftMax, rightMax) - height[i]
leftMax/rightMax 均初始化为height[i]。这样假设左边或者右边没有比自身大的话，答案就是0了。
然后ans累加每次计算出来的resI。

时间复杂度：O(n ^ 2)
空间复杂度：O(1)：
结果发现：此方法仅打败了5%的时间复杂度。

3.Code(https://leetcode.com/submissions/detail/284036549/)

### Solution-2：Dynamic programming
1.上面的核心思路是“对于任意一个i，找到[0, i - 1]和[i + 1, n - 1]内的最大值，然后计算res[i]”
而对于任意一个i，我们找到左边、右边最大值的复杂度都是O(n)。而对于n个i，则是O(n ^ 2)。

2.根据Leetcode84-Largest Rectangle in Histogram的Solution2的启示：
求所有区间[i, j]内的最小值，从O(n ^ 3)优化为O(n ^ 2)，其中区间个数为n ^ 2。也就是说，求每个区间内最值的时间复杂度可以优化为O(1)。

3.leftMax[i]表示：[0, i - 1]内的最大值，那leftMax[i + 1] = max(height[i], leftMax[i]);
也就是说，求当前的最大值，可以通过上一次的结果递推得到。
那么对于所有的i（i在[0, n - 1]），只要从前到后遍历一次即可求得所有的leftMax[i];
同理，rightMax[i]一样。
可以看出，通过空间换时间的方法，将O(n ^ 2)优化为O(n)
这样，我们在O(n)的时间内就解决了本题目。

时间复杂度：O(n)
空间复杂度：O(n)

3.Code(https://leetcode.com/submissions/detail/284056325/)

### Solution-3：Monotonous Stack
1.上面的想法是：对于第i个位置，找出它左边最大的数字a，右边最大的数字b，然后答案就可以计算了。
换个思路，对于位置cur，我们不去找它左边或者右边最大的数字；
我们找第一个比它大的数字：左边下标记为i，右边下标记录为j。
如果找到的话，计算出来这个凹坑的面积，答案累加上这个值。res += min(height[i], height[j]) * (j - i - 1)
此时，由于这块面积已经被累加过了，则可以想象成这个凹坑已经被填上了。遍历了所有的凹坑后，答案就出来了。
自然地，根据739. Daily Temperatures的启示，我们使用单调栈很容易找到一个元素左边和右边第一个比自己大的值。

2.具体算法：如果出现一个新元素，大于栈顶元素，则将栈中元素抛出k个，且每个计算结果，并累加。
如果当前元素小于栈顶元素，则将其压入栈内即可。

时间复杂度：O(n)
空间复杂度：O(n)

3.Code(https://leetcode.com/submissions/detail/284093452/)

### Solution-4：Two Pointers
1.回想下Solution-2，对于任意i，找到leftMax[i]和rightMax[i]。然后就可以计算res[i]的值了。
2.同时，可以发现，当leftMax[i]小于rightMax[i]的时候，res[i]的值由宽度和leftMax[i]决定。
3.left指针指向1号元素，left向右移动。
right指针指向n - 2号元素，right向左移动。
leftMax表示“此时此刻，对于当前元素来说，它左边的最大值”，初始化为height[0]
rightMax表示“此时此刻，对于当前元素来说，它右边的最大值”，初始化为height[n - 1]
则考虑[left, right]区间内的每一个元素：
如果此时leftMax小于rightMax：说明：
a.对于当前元素cur来说（left位置的），这个位置能否存储水，只和height[left]和leftMax有关。
b.对于当前元素cur来说（left位置的），不管我右边的高度分别是多少，只要我右边存在一个位置使得rightMax > leftMax，那么当前位置的存水量就只依赖于leftMax。
此时，ans累加该位置存储的水量，更新leftMax，left后移一位。

如果此时leftMax大于等于rightMax，说明：
a.对于当前元素cur来说（right位置的），这个位置能否存储水，只和height[right]和rightMax有关。
b.对于当前元素cur来说（right位置的），不管我左边的高度分别是多少，只要我左边存在一个位置使得rightMax <= leftMax，那么当前位置的存水量就只依赖于rightMax。
此时，ans累加该位置存储的水量，更新rightMax，right后移一位。

left从左往右，right从右往左，每次循环找出一个位置的答案，然后left++或者right--，直到left > right，此时所有位置的答案都交替计算出来了。

时间复杂度：O(n)
空间复杂度：O(1)

4.Code(https://leetcode.com/submissions/detail/284130675/)






















