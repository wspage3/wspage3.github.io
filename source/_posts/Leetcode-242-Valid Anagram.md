---
title: 242.Valid Anagram
---

## 一、题意
题目链接: [242.Valid Anagram](https://leetcode.com/problems/valid-anagram/)
### 1.Description
【输入】
给定两个字符串s和t。
【输出】
判断这两个字符串是否为：“字母异位词”
所谓“字母异位词”，就是两个单词，组成它们的字母是完全一样的，而不关心其排列顺序。

### 2.Example
Input: s = "anagram", t = "nagaram"
Output: true

### 3.Corner Case
1.lenS不等于lenT的时候，直接return false。
2.s和t中仅包含小写字母。
3.如果输入包含unicode字符的话，该程序的扩展性如何。

## 二、题解
### Solution-1：Sort
1.将两个字符串都排序，然后直接比较字符串是否相等即可。
2.在C++中：
sort(s.begin(), s.end());
return s == t;

时间复杂度：O(n * log n) //n代表字符串的长度
空间复杂度：O(1)

6.Code(https://leetcode.com/submissions/detail/286349008/)

### Solution-2：Hash Table
1.用两个哈希表mapS和mapT，来记录s和t中所有字母出现的次数。
2.然后遍历26个小写字母：
对于任意一个小写字母：
首先，判断其是否在mapS和mapT中同时出现或者同时不出现。
然后，判断其出现的频率是否相等。
如果上述两点有不满足的，直接返回false即可。
3.在C++中，判断其是否出现在unordered_map中可以使用count()，出现返回1，否则返回0。
判断其出现的次数的话，直接比较其key对应的value即可。

时间复杂度：O(n + 26)
空间复杂度：O(26)

4.Code(https://leetcode.com/submissions/detail/286336480/)

### Solution-3：Hash Table with Optimization
1.我们用一个哈希表即可。
2.首先判断lenS, lenT是否相等，不相等的话直接return false。
3.遍历n次，每次：
s[i]对应的value加一；
t[i]对应的value减一；
如果遍历n次后，且两个字符串由相同的字母组成的话，那么所有出现的字符的次数都是0了。
4.遍历整个哈希表，若其个数非0，则return false。

时间复杂度：O(n + 26)
空间复杂度：O(26)

4.Code(https://leetcode.com/submissions/detail/286347847/)

### Solution-4：Use Vector instead of Hash Table
1.我们用一个固定长度为26的数组即可实现Solution-3的思路。
2.在数组中访问赋值的时间复杂度是O(1)。
而理论上哈希表的访问的时间复杂度也是O(1)。
参考：
Hash tables do some complex math in that one operation to get to the value that you want.
Simulating a hash table with an array is quicker because that 
one operation for a lookup is only an offset from the beginning of the array(since you already have the index)
vs more difficult mathy stuff with the hash table.

时间复杂度：O(n + 26)
空间复杂度：O(26)

3.Code(https://leetcode.com/submissions/detail/286348853/)

4.进阶：如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？
解答：使用哈希表而不是固定大小的计数器。想象一下，分配一个大的数组来适应整个 Unicode 字符范围，这个范围可能超过 100万。
哈希表是一种更通用的解决方案，可以适应任何字符范围。