---
title: 287.Find the Duplicate Number
---

## 一、题意
题目链接: [287.Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
### 1.Description
【输入】
给定一个长度为n的整数数组。
其中所有的元素取值都是区间[1, n - 1]的。
根据鸽巢原理，可以证明：至少有两个元素是相等的。
题目保证：数组中的元素刚好有一个存在重复值，但是重复的次数不一定。次数至少为2。
【输出】
找出这个重复的数字。
【参考】
https://zh.wikipedia.org/wiki/%E9%B4%BF%E5%B7%A2%E5%8E%9F%E7%90%86
### 2.Example
Input: [1,3,4,2,2]
Output: 2
### 3.Corner Case
1.数组不允许修改：即不可以排序等操作。
2.空间复杂度要求为O(1)。
3.时间复杂度要求小于O(n ^ 2)。
4.题目保证：数组中的元素刚好有一个存在重复值，但是重复的次数不一定。这个数出现次数至少为2。

## 二、题解
### Solution-1：Brute Force 
1.对于每个i: [0, n - 2]。
遍历j: [i + 1, n - 1]。
如果nums[i] == nums[j]，说明nums[i]就是要找的答案。

时间复杂度：O(n ^ 2)
空间复杂度：O(1)

2.Code(https://leetcode.com/submissions/detail/285661538/)

### Solution-2：Count
1.对于每个i: [0, n - 1]，用一个长度为n的数组v记录其出现的次数。
对于nums[i]，如果v[nums[i]] == 1了，说明nums[i]这个数字之前出现过了。就是答案。
否则的话，将v[nums[i]] = 1。

时间复杂度：O(n)
空间复杂度：O(n)

2.Code(https://leetcode.com/submissions/detail/285662708/)

### Solution-3：Binary Search
1.由于数组长度为n，所以nums[i]的取值范围最大是：[1, n - 1]。此时duplicate出现了两次。

2.我们对[1, n - 1]区间进行二分搜索。每次找到一个mid。对于每一个mid：
遍历nums数组，如果nums[i] <= mid，则cnt加一。
如果此时cnt大于mid了，也就是：数组nums中所有小于等于mid的个数大于了mid。
例如：数组nums中所有小于等于5的数字的个数大于5。说明duplicate在范围[1, 5]内。
此时在[left, mid]中搜索。

3.如果此时cnt小于等于mid，则说明duplicate在[mid + 1, right]中。

4.考虑最后一次while循环，left = right - 1。mid = left。
判断left位置的合法性：
如果left位置合法，则left = mid + 1 = right。
如果left位置不合法，则right = mid = left。
不管哪种情况，循环都走不下去了，都有left == right。而且两者当前的值就是非法的那个值。
此时return left(或right)即可。

时间复杂度：O(n * log n)
空间复杂度：O(1)

5.Code(https://leetcode.com/submissions/detail/285704963/)

6.证明：
Let count be the number of elements in the range 1 .. mid.

If count > mid, then there are more than mid elements in the range 1 .. mid and thus that range contains a duplicate.

If count <= mid, then there are n+1-count elements in the range mid+1 .. n. That is, at least n+1-mid elements in a range of size n-mid. Thus this range must contain a duplicate.

### Solution-4：Two Pointers
1.观察：长度为n的nums数组中，所有元素的大小均在范围：[1, n - 1]中。
也就是每个元素的值都可以作为nums的index。

2.https://leetcode.com/problems/linked-list-cycle-ii/
类似的，我们把nums数组想象成一个特殊的链表。
例如：[3,1,3,4,2]
index为0的元素的值为3
index为1的元素的值为1
index为2的元素的值为3
index为3的元素的值为4
index为4的元素的值为2
可以发现，对于一个目标值3，它有两个对应的index，而这个3就是要找的答案duplicate。

3.回忆链表中有环的情况：
如果一个链表中某一个节点N，存在节点A，B，使得：
A -> next == N
B -> next == N
那么这个链表中一定存在环。且第一次出现环的点就是N。我们可以用【快慢指针法+弗洛伊德探测环路方法】找出该N的位置。

4.同样的，我们把上面的nums数组想象成一个链表。由于nums数组中存在duplicate，所以至少两个不同的index，对应的值都是duplicate。
例如：[1,3,4,2,3]
0 -> 1 -> 3 -> 2 -> 4 -> 3 -> 2 -> 4 -> 3......

5.根据【快慢指针法+弗洛伊德探测环路方法】
slow和fast同时从0出发。
slow: slow = nums[slow]
fast: fast = nums[nums[fast]]
当slow和fast相等的时候，break掉。
此时head从0出发。
head: head = nums[head]
slow: slow = nums[slow]
如果此时head == slow，那么这个位置就是要找的duplicate。

6.证明：fast和slow从同一起点出发，fast的速度是slow的2倍。那么当它们相遇时，fast走过的路程是slow走过的2倍。
记起点到环点的距离为s1，环点到meet点的距离为s2，meet点再到环点的距离为s3。
当slow和fast相遇的时候，有：2(s1 + s2) = s1 + s2 + k(s2 + s3)。其中k >= 1。
即：s1 + s2 = ks2 + ks3
当k为1的时候，s1 == s3
当k为2的时候，s1 == s2 + 2s3 = s3 + (s2 + s3)
当k为3的时候，s1 == 2s2 + 3s3 = s3 + 2(s2 + s3)

可以看出，当head和slow同时出发，它们相遇的时候，一定在环点。


时间复杂度：O(n)
空间复杂度：O(1)

5.Code(https://leetcode.com/submissions/detail/285811401/)

