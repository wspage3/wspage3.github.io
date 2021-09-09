---
title: 406.Queue Reconstruction by Height
---

## 一、题意
题目链接: [406.Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)
### 1.Description
【输入】
给定n组数据代表一个队伍的情况。每一组代表一个人的“情况”。
一组数据有两个字段：
1.我的高度。
2.我前面有多少个人，他们的高度大于等于我。
例如：[7,1]代表：我的高度是7m，我前面还有一个人，他的高度大于等于7m。
【输出】
将这个队伍从无序排列到有序。
使得队伍的排序满足所有人的“情况”。

### 2.Example
Input:（原始无序队伍）
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:（有序队伍）
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]

### 3.Corner Case
1. n < 1100
2. h：高度是相对的，所以不考虑其正负，是否为0等。任意整数即可。浮点数也行。（本题目给出的是int）
3. k：前面不低于自己的个数k应该 >= 0。

## 二、题解
### Solution-1：Greedy
1.队伍的长度为n，也就是有n个人。
例如：([7,0], [4,4], [7,1], [5,0], [6,1], [5,2])：
现在需要对它们重新排列，新建长度为n的数组ans用于存放结果。(-1, -1, -1, -1, -1, -1)
对于每个元素cur，假设其前面有k个人。我们在前面预留k个【坑】即可，然后将cur找一个坑。对于坑位的所有数字，都大于等于当前元素cur。

2.举个例子：
a.首先考虑最矮的那个：高度为4；它前面有4个人。所以：从前到后遍历ans，找4个空坑（index为0，1，2，3）。当前元素占用坑位（index为4）
  ans数组变为：(-1, -1, -1, -1, 4, -1)
b.然后考虑第二矮的那个：发现有两个人高度都是5：
  假设[5,2]在[5,0]前面，对于[5,0]来说，不允许自身前面有大于等于5的出现，出现矛盾。
  所以当我们从前到后找坑时，对于h相同的若干个人，需要先考虑k值较小的。
  考虑：[5, 0]：从前到后遍历ans，找0个坑。当前元素占用坑位（index为0）
  (5, -1, -1, -1, 4, -1)
  考虑：[5, 2]：从前到后遍历ans，找2个坑。当前元素占用坑位（index为2）
  (5, -1, 5, -1, 4, -1)
  注意：此时对于第二个5来说，由于第一个5的存在，所以第一个5也算一个【坑】。
c.考虑：[6, 1]：从前到后遍历ans，找1个坑。当前元素占用坑位（index为3）
  (5, -1, 5, 6, 4, -1)
d.考虑：[7, 0]：从前到后遍历ans，找0个坑。当前元素占用坑位（index为1）
  (5, 7, 5, 6, 4, -1)
e.考虑：[7, 1]：从前到后遍历ans，找0个坑。当前元素占用坑位（index为1）
  (5, 7, 5, 6, 4, 7)
答案：([5,0], [7,0], [5,2], [6,1], [4,4], [7,1])

3.具体实现：
a.调用sort函数，自定义比较器：按照h从小到大；若h相同，k从小到大。
b.遍历[0, n - 1]，考虑每个candidate：
c.对于每个candidate，需要遍历ans的[0, n - 1]，找到自己的【专属坑位】

时间复杂度：O(n * n)
空间复杂度：O(n * 2) //需要返回的结果。n行2列。

4.Code(https://leetcode.com/submissions/detail/287233156/)
```C++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        int n = people.size();
        vector<vector<int>> ans(n, vector<int>(2, -1));
        //1.按照高度从小到大排序。
        sort(people.begin(), people.end(), [](const vector<int>& a, const vector<int>& b) {
            if (a[0] < b[0]) {
                return true;
            } else if (a[0] > b[0]) {
                return false;
            } else {
                return a[1] < b[1];
            }
        });
        //2.依次考虑每个candidate，其高度是最小的。
        for (int i = 0; i <= n - 1; i++) {
            //3.对于该candidate，在ans中找一个合适的位置，将其放进去。
            int candidateHeight = people[i][0];
            int preCnt = people[i][1];
            int cnt = 0;
            for (int j = 0; j <= n - 1; j++) {
                //preCnt代表：candidate前面有几个元素的高度大于等于candidate本身
                //如果1：该位置还没有被占用的话，是比自身高度大的元素。
                //如果2：该位置的高度和当前candidate的高度相等，则将当前位置也累加到cnt
                if (ans[j][0] == candidateHeight || ans[j][1] == -1) {
                    if (cnt == preCnt) {
                        ans[j][0] = candidateHeight;
                        ans[j][1] = preCnt;
                        break;
                    }
                    cnt++;
                }
            }
        }
        return ans;
    }
};
```

### Solution-2：Greedy
1.在Solution-1中，我们是先给“矮的人”安排坑位，因为知道它前面有多少个比自身大于等于的。直接预留对应数目的坑位即可。

2.现在考虑先给“高的人”安排坑位：
考虑：[7, 0]，由于k是0，将其放入队列中第一个。ans变成：([7, 0])
考虑：[7, 1]，由于k是1，所以放在[7, 0]的后面。ans变成：([7, 0], [7, 1])
考虑：[6, 1]，由于k是1，所以放在[7, 0]的后面。ans变成：([7, 0], [6, 1], [7, 1])
考虑：[5, 0]，由于k是0，所以放在队列中第一个。ans变成：([5, 0], [7, 0], [6, 1], [7, 1])
考虑：[5, 2]，由于k是2，所以放在[7, 0]的后面。ans变成：([5, 0], [7, 0], [5, 2], [6, 1], [7, 1])
考虑：[4, 4]，由于k是4，所以放在[6, 1]的后面。ans变成：([5, 0], [7, 0], [5, 2], [6, 1], [4, 4], [7, 1])

3.观察可以发现：高度位h的元素进入队伍后，不管其怎么插队，都不会影响高度大于h的元素的“合法性”。
因为每个元素只关心比在自己前面，且高度大于等于自己的元素个数。

时间复杂度：O(n ^ 2)
空间复杂度：O(n * 2) //需要返回的结果。n行2列。

参考(https://leetcode.com/problems/queue-reconstruction-by-height/discuss/89359/Explanation-of-the-neat-Sort%2BInsert-solution)

6.Code(https://leetcode.com/submissions/detail/287237749/)
```C++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        int n = people.size();
        vector<vector<int>> ans;
        //1.按照高度从小到大排序。
        sort(people.begin(), people.end(), [](const vector<int>& a, const vector<int>& b) {
            if (a[0] < b[0]) {
                return false;
            } else if (a[0] > b[0]) {
                return true;
            } else {
                return a[1] < b[1];
            }
        });
        //2.依次考虑每个candidate，其高度是最大的。
        for (int i = 0; i <= n - 1; i++) {
            //3.对于该candidate，插入ans中。
            ans.insert(ans.begin() + people[i][1], people[i]);
        }
        return ans;
    }
};
```