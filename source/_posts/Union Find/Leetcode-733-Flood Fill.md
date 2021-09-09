---
title: Leetcode-733.Flood Fill
categories: 
- [Leetcode]
- [数据结构与算法]
tags: 
- BFS
- DFS
- Union Find
---

## 一、题意

### 0、题目链接
[733.Flood Fill](https://leetcode.com/problems/flood-fill/)

### 1、Description
【输入】
给定m * n的二维数组image，代表一张图片；
每个元素代表图片的一个像素点，每个像素点都有一个颜色值。数据类型为整数，取值范围为：[0, 65535]。
给定一个坐标p：(sr, sc)，代表图片中的某个像素点；
给定一个整数newColor，代表某种颜色；
【输出】
输出“Flood Fill”后的新图片；
【Flood Fill】
从坐标p出发，找出image中所有符合条件的像素点，将其颜色值设置为newColor；
符合条件：与p的颜色相同；且与p直接或间接相邻；
【要求】
无；

### 2、Example
image = 
[
    [1,1,1],
    [1,1,0],
    [1,0,1]
]
sr = 1, sc = 1, newColor = 2
output = 
[
    [2,2,2],
    [2,2,0],
    [2,0,1]
]

<!-- more -->

### 3、Corner Case
1. 1 <= m、n <= 50
2. 初始坐标p一定在image内

## 二、题解

### 0、思考
1. 求image内某些点的“联通性”，可以使用并查集来解决；

2. 也可以从p出发，直接进行BFS/DFS进行搜索；

### 1、Solution-1：BFS Using Queue
1. 先将p放入queue；

2. 每次从queue取出一个元素：
将其value设置为newColor；
考虑其邻居，若其符合条件（颜色为target），则将其放入queue；

3. 直到队列为空，所有符合条件的元素都被考虑过了。

4. 不需要判重：因为访问过的结点被赋了新值，下一次就不满足条件了，不会再次进入queue；

5. Code(https://leetcode.com/submissions/detail/345416148/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
private:
    bool isInImage(int m, int n, int r, int c) {
        if (r >= 0 && r <= m - 1 && c >= 0 && c <= n - 1) {
            return true;
        }
        return false;
    }
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) {
            return image;
        }
        
        int m = image.size();
        int n = image[0].size();
        int target = image[sr][sc];
        
        vector<vector<int>> clone(image);
        
        queue<pair<int, int>> q;
        q.push(make_pair(sr, sc));
        while (!q.empty()) {
            int curR = q.front().first;
            int curC = q.front().second;
            q.pop();
            clone[curR][curC] = newColor;
            if (isInImage(m, n, curR - 1, curC) && clone[curR - 1][curC] == target) {
                q.push(make_pair(curR - 1, curC));
            }
            if (isInImage(m, n, curR, curC - 1) && clone[curR][curC - 1] == target) {
                q.push(make_pair(curR, curC - 1));
            }
            if (isInImage(m, n, curR, curC + 1) && clone[curR][curC + 1] == target) {
                q.push(make_pair(curR, curC + 1));
            }
            if (isInImage(m, n, curR + 1, curC) && clone[curR + 1][curC] == target) {
                q.push(make_pair(curR + 1, curC));
            }
        }
        
        return clone;
    }
};
```

### 2、Solution-2：DFS
1. 从p出发，进行DFS；

2. 遍历到某个位置，进行合法性检查：在image外，或颜色不为target，则不考虑该点；

3. 合法的话：将其颜色设置为newColor；考虑其四个邻居；

4. 不需要判重：因为访问过的结点被赋了新值，下一次就不满足条件了，直接return出去；

5. Code(https://leetcode.com/submissions/detail/345773024/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
private:
    void dfs(vector<vector<int>>& clone, int sr, int sc, int newColor, int target) {
        int m = clone.size();
        int n = clone[0].size();
        if (sr < 0 || sr > m - 1 || sc < 0 || sc > n - 1 || clone[sr][sc] != target) {
            return;
        }
        clone[sr][sc] = newColor;
        dfs(clone, sr - 1, sc, newColor, target);
        dfs(clone, sr, sc - 1, newColor, target);
        dfs(clone, sr, sc + 1, newColor, target);
        dfs(clone, sr + 1, sc, newColor, target);
    }
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) {
            return image;
        }
        vector<vector<int>> clone(image);
        dfs(clone, sr, sc, newColor, image[sr][sc]);
        return clone;
    }
};
```

### 3、Solution-3：Union Find
1. 遍历image，处理所有结点的联通性：若其相邻、且颜色相同，则进行联通；

2. 找到p所在的“圈子”，记录为x；

3. 再次遍历image，若image[i][j]的“圈子”为x，代表其与p直接或间接相邻。将其赋值newColor；

4. Code(https://leetcode.com/submissions/detail/345778493/)
AC
时间复杂度：O(m * n)
空间复杂度：O(m * n)
```C++
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) {
            return image;    
        }
        
        int m = image.size();
        int n = image[0].size();
        UnionFind uf(m * n);
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                int idx = i * n + j;
                int color = image[i][j];
                if (i + 1 <= m - 1 && image[i + 1][j] == color) {
                    uf.Union(idx, (i + 1) * n + j);
                }
                if (j + 1 <= n - 1 && image[i][j + 1] == color) {
                    uf.Union(idx, i * n + j + 1);
                }
            }
        }
        
        vector<vector<int>> clone(image);
        int x = uf.Find(sr * n + sc);
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                int idx = i * n + j;
                if (uf.Find(idx) == x) {
                    clone[i][j] = newColor;
                }   
            }
        }
        
        return clone;
    }
};
```