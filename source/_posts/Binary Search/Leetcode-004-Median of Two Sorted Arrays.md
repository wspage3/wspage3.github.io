---
title: Leetcode-004.Median of Two Sorted Arrays
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- Array
- Binary Search
---

## 一、题意

### 0、题目链接
[004.Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

### 1、Description
【输入】
给定两个非空有序数组nums1、nums2，长度分别为m、n；
【输出】
输出nums1、nums2整体的中位数；
【要求】
时间复杂度为O(log(m + n))；

### 2、Example
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3) / 2 = 2.5

<!-- more -->

### 3、Corner Case
1. n1、n2均为正数；

## 二、题解

### 0、思考
1. 中位数：又称中值，统计学中的专有名词，是按顺序排列的一组数据中居于中间位置的数。
若n为奇数：则中位数就是中间那个数。如：[1, 2, 3] x = 2；
若n为偶数，则中位数是中间那两个数的平均值。如：[1, 2, 3, 4] x = 2.5；

### 1、Solution-1：Brute Force
1. 将nums1和nums2进行merge，结果存放在一个新的数组v中，大小为m + n。

2. 然后计算v的中位数。

3. Code(https://leetcode.com/submissions/detail/327945330/)
AC
时间复杂度：O(m + n)
空间复杂度：O(m + n)
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        
        vector<int> v;
        int i = 0;
        int j = 0;
        while (i <= m - 1 && j <= n - 1) {
            if (nums1[i] <= nums2[j]) {
                v.push_back(nums1[i]);
                i++;
            } else {
                v.push_back(nums2[j]);
                j++;
            }
        }
        if (i == m) {
            for (; j <= n - 1; j++) {
                v.push_back(nums2[j]);
            }
        } else {
            for (; i <= m - 1; i++) {
                v.push_back(nums1[i]);
            }
        }
        int len = m + n;
        double ans;
        if (len % 2 == 1) {
            ans = v[len / 2];
        } else {
            ans = (v[len / 2] + v[len / 2 - 1]) / 2.0;
        }
        return ans;
    }
};
```

4. 优化上述空间复杂度：
不使用大小为m + n的数组v来保存所有的数字。

5. 当len(m + n)为奇数时：如7，需要遍历4次，得到nums[3]，就是中位数了。
当len(m + n)为偶数时：如8，需要遍历5次，得到nums[3]和nums[4]，它们俩的平均数就是中位数了。
综上所述，需要遍历cnt次，cnt = len / 2 + 1;

6. 每次我们选择nums1、nums2中较小的那个数字。

7. Code(https://leetcode.com/submissions/detail/327956091/)
AC
时间复杂度：O(k) = O(m + n) // 我们找第k小的元素，只要循环k次就行。这里k = len / 2 + 1。
空间复杂度：O(1)
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();        
        int len = m + n;        
        
        int left = -1; 
        int right = -1; 
        int i = 0;
        int j = 0;
        for (int num = 1; num <= len / 2 + 1; num++) {
            left = right;
            if (j == n || (i <= m - 1 && nums1[i] <= nums2[j])) {
                right = nums1[i];
                i++;
            } else {
                right = nums2[j];
                j++;
            }
        }
                
        double ans;
        if (len % 2 == 1) {
            ans = right;
        } else {
            ans = (left + right) / 2.0;
        }
        return ans;
    }
};
```

### 2、Solution-2：Binary Search
1. 当len(m + n)为奇数时：如7，需要遍历4次，得到nums[3]（第4小的数字），就是中位数了。
当len(m + n)为偶数时：如8，需要遍历5次，得到nums[3]（第4小的数字）和nums[4]（第5小的数字），它们俩的平均数就是中位数了。

2. getKth(nums1, left1, right1, nums2, left2, right2, k)：得到nums1、nums2数组合并后，第k小的数字。

3. 举个例子：
nums1：[1, 3, 4, 9]
nums2：[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
len = 14，我们需要找第7、第8小的数字，取平均数即可。

4. 假设k = 8
先找出num1、nums2各自第k / 2的数字：9和4。
因为4 < 9，说明nums2中的前k / 2个数字(1, 2, 3, 4)，都不可能是nums1、nums2中第k小的数字，将其舍弃。
因为：[1, 3, 4, 9]、[1, 2, 3, 4]中，9是最大的数字，也就是第8小的数。而4 < 9，肯定不是nums1、nums2中第8小的数。将其舍弃。
然后在[1, 3, 4, 9]、[5, 6, 7, 8, 9, 10]中寻找第4小的数字：5。

5. 假设k = 7
先找出num1、nums2各自第k / 2的数字：4和3。
因为3 < 4，说明nums2中的前k / 2个数字(1, 2, 3)，都不可能是nums1、nums2中第k小的数字，将其舍弃。
因为：[1, 3, 4]、[1, 2, 3]中，4是最大的数字，也就是第6小的数。而3 < 4，肯定不是nums1、nums2中第7小的数。舍弃。
然后在[1, 3, 4, 9]、[4, 5, 6, 7, 8, 9, 10]中寻找第4小的数字：4。

6. 所以上述中位数：4.5。

7. Code(https://leetcode.com/submissions/detail/328076534/)
AC
时间复杂度：O(log k) = O(log(m + n))
空间复杂度：O(1)
```C++
class Solution {
private:
    int getKth(vector<int>& nums1, int left1, int right1, vector<int>& nums2, int left2, int right2, int k) {
        int len1 = right1 - left1 + 1;
        int len2 = right2 - left2 + 1;
        // 0. 保证nums1是较小的：
        if (len1 > len2) {
            return getKth(nums2, left2, right2, nums1, left1, right1, k);
        }
        // 1. 考虑一个数组为空的情况，直接返回答案即可：
        // 因为题目保证m、n均为正数。而每次只会改变一个数组的搜索空间，不会使得两个数组的搜索空间都为空。
        if (len1 == 0) {
            return nums2[left2 + k - 1];
        }
        // 2. 考虑k为1的情况，直接返回答案即可：
        if (k == 1) {
            return min(nums1[left1], nums2[left2]);
        }
        // 3. 获取nums1和nums2各自【第k / 2个】元素的index：
        int idx1 = min(left1 + k / 2 - 1, right1);
        int idx2 = min(left2 + k / 2 - 1, right2);
        // 4. 通过比较两个值的大小，决定舍弃哪一部分搜索空间：
        int dropCnt;
        if (nums1[idx1] < nums2[idx2]) {
            dropCnt = idx1 - left1 + 1;
            return getKth(nums1, idx1 + 1, right1, nums2, left2, right2, k - dropCnt);
        } else {
            dropCnt = idx2 - left2 + 1;
            return getKth(nums1, left1, right1, nums2, idx2 + 1, right2, k - dropCnt);
        }
    }
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();        
        int len = m + n;
        double ans;
        if (len % 2 == 1) {
            ans = getKth(nums1, 0, m - 1, nums2, 0, n - 1, (len + 1) / 2);
        } else {
            ans = (getKth(nums1, 0, m - 1, nums2, 0, n - 1, len / 2) + getKth(nums1, 0, m - 1, nums2, 0, n - 1, len / 2 + 1)) / 2.0;
        }
        return ans;
    }
};
```

### 3、Solution-3：Binary Search
1. 参考视频(https://www.youtube.com/watch?v=LPFhl65R7ww)

2. 我们期望将m + n个元素分为两部分。左边所有元素都小于等于右边的元素。
当m + n为偶数时：如8，期望左边cnt为4。
当m + n为奇数时：如9，期望左边cnt为5。
cnt = (m + n + 1) / 2;

3. 对长度为m的nums1数组进行cut，使得分成左右两部分，一共有以下m + 1种cut方法：
{0, m}, {1, m - 1}, {2, m - 2}, ..., {m, 0}
其中左边数目为：[0, m]

4. 当nums1进行cut后，其左边元素个数就确定了，为cntLeft1。
可以得知nums2的左边元素个数：cntLeft2 = cnt - cntLeft1。

5. 然后校验该种方案的合法性。

6. Code(https://leetcode.com/submissions/detail/331365460/)
AC
时间复杂度：O(log(min(m, n)))
空间复杂度：O(1)
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        
        // 1. 保证nums1是较短的那个数组，从而对其进行二分，使得时间复杂度较低。
        if (m > n) {
            return findMedianSortedArrays(nums2, nums1);
        }
 
        int cnt = (m + n + 1) / 2;
 
        int low = 0;
        int high = m;
        double ans;
        // 2. 只要还有cut的可能性，就进行搜索
        while (low <= high) {           
            // 2-1：选择一种cut方案，使得：nums1左边个数为cntLeft1，nums2左边个数为cntLeft2。
            int cntLeft1 = low + (high - low) / 2;
            int cntLeft2 = cnt - cntLeft1;
            
            // 2-2：校验这种方案的合法性：看是否左边的数字都比右边的数字小。
            int maxLeft1 = cntLeft1 == 0 ? INT_MIN : nums1[cntLeft1 - 1];
            int maxLeft2 = cntLeft2 == 0 ? INT_MIN : nums2[cntLeft2 - 1]; 
            int minRight1 = cntLeft1 == m ? INT_MAX : nums1[cntLeft1];
            int minRight2 = cntLeft2 == n ? INT_MAX : nums2[cntLeft2];
            // 2-3：如果合法，则返回答案。
            if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
                if ((m + n) % 2 == 1) {
                    ans = max(maxLeft1, maxLeft2);
                } else {
                    ans = (max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2.0;
                }
                return ans;
            } else if (maxLeft1 > minRight2) {
                // 对于nums1来说，当前cut的位置数字较大，应该向左搜索。
                high = cntLeft1 - 1;
            } else {
                // nums1向右搜索
                low = cntLeft1 + 1;
            }
        }
        return -1; // cannot execute
    }
};
```
