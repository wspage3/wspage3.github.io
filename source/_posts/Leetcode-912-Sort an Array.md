---
title: 912.Sort an Array
---

## 一、题意
题目链接: [912.Sort an Array](https://leetcode.com/problems/sort-an-array/)
### 1.Description
【输入】
给定一个长度为n的整数数组nums。
【输出】
返回一个数组，使其是nums升序后的数组。

### 2.Example
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]

### 3.Corner Case
1. 1 <= nums.length <= 50000
2. -50000 <= nums[i] <= 50000

## 二、题解
### Solution-1：Bubble Sort
1.通过将更大的元素“沉入”底端，来使得数组有序。

2.长度为n的数组nums，最多需要(n - 1)轮。
i的取值范围：[1, n - 1]
每一轮，找出一个最大的值，将其放在末尾。
例如n为3时，
第1轮将数组中最大的数字放在末尾(n - 1)。
第二轮将数组中第二大的数字放在(n - 2)位置上。
排序完成。

3.对于每轮，从位置0开始考虑，且比上一轮少考虑1个元素。
第一轮，考虑：[0, n - 1]区间：0 <= j <= n - 1 - 1
第二轮，考虑：[0, n - 2]区间，0 <= j <= n - 2 - 1
所以j的取值范围是：[0, n - i - 1]

4.稳定性分析：由于比较的时候，只有前一个元素大于后一个元素，才会交换值。
若相等，不会交换值，所以Bubble Sort是稳定的。

5.优化：如果某一轮，发现没有进行swap的操作，说明数组整体上已经是有序的了，直接break掉即可。

时间复杂度：O(n ^ 2)
空间复杂度：O(1) 

参考：(https://www.jianshu.com/p/1458abf81adf)

5.Code(https://leetcode.com/submissions/detail/286810018/)
```C++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> ans(nums);
        int n = ans.size();
        for (int i = 1; i <= n - 1; i++) {
            bool flag = false;
            for (int j = 0; j <= n - i - 1; j++) {
                if (ans[j] > ans[j + 1]) {
                    swap(ans[j], ans[j + 1]);
                    flag = true;
                }
            }
            if (flag == false) {
                break;
            }
        }
        return ans;
    }
};
```

### Solution-2：Selection Sort
1.考虑[0, n - 2]上的每一个元素。i: [0, n - 2]

2.对于任意i，j从i+1开始，直到nums最后一个元素为止。j: [i + 1, n - 1]。
如果某个nums[j] < nums[i]，则swap两个值。
这样下来，对于任意i，O(n)的时间内，找出[i, n - 1]内最小的值，并将其赋值给nums[i]。

3.和Bubble的区别：
Bubble Sort：对于第i轮，考虑区间[0, n - i]，在这个区间上从前到后通过“两两比较”，将【较大】的值移动到最后，使得本区间内【最大】的元素放在最后面。问题规模减一。
Select Sort：对于第i个元素，考虑区间[i, n - 1]，在这个区间上所有元素“都和nums[i]比较”，找出一个【最小】的值，然后和nums[i]交换，使得本区间内【最小】的元素放在最前面。问题规模减一。

4.稳定性分析：存在着不相邻元素之间的互换，所以是不稳定的。
例如：[5, 1, 5, 4]
i = 0: [1, 5, 5, 4]
i = 1: [1, 4, 5, 5] //此时将第一个5和4交换，导致第一个5出现在第二个5的后面，所以不稳定
i = 2: [1, 4, 5, 5]

时间复杂度：O(n ^ 2)
空间复杂度：O(1) 

参考：(https://www.jianshu.com/p/51100da14cc2)

5.Code(https://leetcode.com/submissions/detail/286813272/)
```C++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> ans(nums);
        int n = ans.size();
        for (int i = 0; i <= n - 2; i++) {
            for (int j = i + 1; j <= n - 1; j++) {
                if (ans[j] < ans[i]) {
                    swap(ans[i], ans[j]);
                }
            }
        }
        return ans;
    }
};
```

### Solution-3：Insertion Sort
1.插入排序的基本操作就是：将一个数据插入到已经排好序的有序数据中，从而得到一个新的、个数加一的有序数据。

2.开始可以认为nums[0]是有序的。
  考虑所有无序的元素，每次考虑一个i：[1, n - 1]。

3.对于每个元素nums[i](cur)，将其插入到区间[0, i - 1]中，使得这个有序列表数量加一。从而使得无序列表规模减一。

4.j从i - 1开始：
若j是合法的index：j >= 0
而且nums[j] > cur, 将此时的nums[j]放在数组的后一个位置上。同时j--继续向前寻找。
直到找到一个位置，使得nums[j] <= cur为止。此时将cur放入nums[j + 1]即可。

5.稳定性分析：可以发现，对于任意一个nums[i]来说，若其左边的有序列表中存在和它相同的一个元素。
则当j往前遍历的时候，发现nums[j] == nums[i]了，此时循环结束。将nums[j + 1]赋值为nums[i]。
也就是说，若两个元素相等，它们的先后顺序还是不会变的，所以Insertion Sort是稳定的。

时间复杂度：O(n ^ 2)
空间复杂度：O(1) 

6.Code(https://leetcode.com/submissions/detail/286836860/)
```C++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> ans(nums);
        int n = ans.size();
        for (int i = 1; i <= n - 1; i++) {
            int cur = ans[i];
            int j = i - 1;
            while (j >= 0 && ans[j] > cur) {
                ans[j + 1] = ans[j];
                j--;
            }
            ans[j + 1] = cur;
        }
        return ans;
    }
};
```

### Solution-4：Shell Sort
0.参考：(https://www.bilibili.com/video/av15961896?from=search&seid=680452632492195488)

1.希尔排序是一种插入排序算法，它出自D.L.Shell，因此而得名。Shell排序又称作缩小增量排序。Shell排序的执行时间依赖于增量序列。
  Shell本人给出的一个增量序列为：n / 2 ... n / 4 ... n / 8 ... 1

2.为了减少数据的移动次数，在初始序列较大时取较大的步长，通常取序列长度的一半，此时只有两个元素比较，交换一次；
  之后步长依次减半直至步长为1，即为插入排序，由于此时序列已接近有序，故插入元素时数据移动的次数会相对较少，效率得到了提高。

3.在Insertion Sort外层嵌套一层循环，代表增量序列递减。
[5, 3, 6, 7, 3, 2, 7, 8, 0], n = 9
gap：4 -> 2 -> 1
当gap为1的时候，就是传统的Insertion Sort。

4.具体情况：
gap = 4 : [0, 2, 6, 7, 3, 3, 7, 8, 5]
gap = 2 : [0, 2, 3, 3, 5, 7, 6, 8, 7]
gap = 1 : [0, 2, 3, 3, 5, 6, 7, 7, 8] OK

5.稳定性分析：由于多次插入排序，我们知道一次插入排序是稳定的，不会改变相同元 素的相对顺序。
但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱；
[21, 25(1), 49, 25(2), 16, 8]  n = 6
n = 3 : [21, 16, 8, 25(2), 25(1), 49] //此时两个25的顺序已经变化了
n = 1 : [8, 16, 21, 25(2), 25(1), 49] //说明希尔排序是不稳定的

时间复杂度：O(n ^ 1.5) // 普遍认为，未证明
空间复杂度：O(1) 
AC了！！！

6.Code(https://leetcode.com/submissions/detail/286841644/)
```C++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> ans(nums);
        int n = ans.size();
        for (int gap = n / 2; gap >= 1; gap /= 2) {
            
            for (int i = gap; i <= n - 1; i++) {
                int cur = ans[i];
                int j = i - gap;
                while (j >= 0 && ans[j] > cur) {
                    ans[j + gap] = ans[j];
                    j -= gap;
                }
                ans[j + gap] = cur;
            }
            
        }
        return ans;
    }
};
```

### Solution-5：Counting Sort
1.计数排序：典型的空间换取时间的操作。
2.通过统计数组中每个数出现的次数，并将其存起来。然后从前到后写到ans数组中即可。
3.步骤：
首先，求出nums数组中最大值maxElement和最小值minElement。
然后，申请(maxElement - minElement + 1)长度的counting数组。
其次，遍历nums数组，若nums[i]出现了1次，则将counting[nums[i] - minElement]累加一次。
最后，通过构造出来的counting数组还原原数组。

时间复杂度：O(n + m) // n是数组长度，m是数组中(最大值 - 最小值 + 1)
空间复杂度：O(m) 

4.局限性：只对(maxElement - minElement + 1)数值较小的情况有效，否则申请太大的内存，十分浪费；
只对整数有效。

5.稳定性分析：稳定。

6.Code(https://leetcode.com/submissions/detail/286865573/)
```C++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> ans(nums);
        int n = ans.size();
        int minElement = ans[0];
        int maxElement = ans[0];
        for (int i = 1; i <= n - 1; i++) {
            minElement = min(minElement, ans[i]);
            maxElement = max(maxElement, ans[i]);
        }
        int size = maxElement - minElement + 1;
        vector<int> counting(size, 0);
        for (int i = 0; i <= n - 1; i++) {
            counting[ans[i] - minElement]++;
        }
        int cur = 0;
        for (int i = 0; i <= size - 1; i++) {
            while (counting[i]--) {
                ans[cur++] = i + minElement;
            }
        }
        return ans;
    }
};
```

### Solution-6：Quick Sort
1.Quick Sort是对Bubble Sort的一种改进：
它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小。
然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

2.每次考虑子区间[left, right]。开始考虑nums数组的[0, n - 1]。
选择nums[left]作为target，经过O(n)的遍历，使得在区间[left, right]上：
target（位置i）左边的值都小于等于target。
target（位置i）右边的值都大于它。
然后分别递归解决左子区间：[left, i - 1]和右子区间[i + 1, right]的排序问题。

3.稳定性分析：不稳定

时间复杂度：O(n * log n)
空间复杂度：O(log n) // 递归栈的开销
最好的情况下，即快速排序的每一趟排序都将元素序列均匀地分割成长度相近的两个子表，所需栈的最大深度为log2(n+1)；
最坏的情况下，栈的最大深度为n。

参考：(https://www.jianshu.com/p/c1a05c50a75c)

6.Code(https://leetcode.com/submissions/detail/287640003/)
```C++
class Solution {
private:
    void quickSort(vector<int>& nums, int left, int right) {
        if (left >= right) {
            return;
        }
        int target = nums[left];
        int i = left, j = right;
        //i一定小于j，至少loop一次
        while (i < j) {
            //指针j寻找一个【小于等于】target的值
            while (i < j && nums[j] > target) {
                j--;
            }
            //指针i寻找一个【大于】target的值
            while (i < j && nums[i] <= target) {
                i++;
            }
            if (i < j) {
                swap(nums[i], nums[j]);
            }
        }
        //此时i一定等于j，且[left, i]所有的数都小于等于target
        swap(nums[left], nums[i]);
        quickSort(nums, left, i - 1);
        quickSort(nums, i + 1, right);
        return;
    }
public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> ans(nums);
        int n = ans.size();
        quickSort(ans, 0, n - 1);
        return ans;
    }
};
```

### Solution-7：Merge Sort
1.Merge Sort是一种基于“分治法”思想的排序方法。核心思想如下（Top-Down）：
将一个长度为n的数组分为左右两部分，递归对这两个子问题求解。然后将两个子问题的解合并拼出本问题的解。

2.对于在区间[left, right]上排序。mid = left + (right - left) / 2;
首先：递归调用mergeSort(nums, temp, left, mid);
             mergeSort(nums, temp, mid + 1, right);
其次：merge两部分：
left指针指向左子数组的最后一个元素。
right指针指向右子数组的最后一个元素。
每次选择一个较大的元素，存放在temp[cur--]中（cur初始化为right）。
直到这(right - left + 1)个元素都放在了temp数组中。
最后将temp数组中对应这个范围的有序数据依次拷贝回nums数组中。

3.稳定性分析：稳定

时间复杂度：O(n * log n)
空间复杂度：O(n) 

4.Code(https://leetcode.com/submissions/detail/287087746/)
```C++
class Solution {
private:
    void mergeSort(vector<int>& nums, vector<int>& temp, int left, int right) {
        if (left >= right) {
            return;
        }
        int mid = left + (right - left) / 2;
        mergeSort(nums, temp, left, mid);
        mergeSort(nums, temp, mid + 1, right);
        
        int cur = right;
        int p1 = mid, p2 = right;
        while (p1 >= left && p2 >= mid + 1) {
            if (nums[p2] >= nums[p1]) {
                temp[cur--] = nums[p2--];
            } else {
                temp[cur--] = nums[p1--];
            }
        }
        if (p1 < left) {
            while (p2 >= mid + 1) {
                temp[cur--] = nums[p2--];
            }
        } else {
            while (p1 >= left) {
                temp[cur--] = nums[p1--];
            }
        }
        for (int i = left; i <= right; i++) {
            nums[i] = temp[i];
        }
        return;
    }
public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> temp(nums.size());
        mergeSort(nums, temp, 0, nums.size() - 1);
        return nums;
    }
};
```























