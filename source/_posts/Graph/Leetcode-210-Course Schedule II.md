---
title: 210.Course Schedule II
---

## 一、题意
题目链接: [210.Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
### 1.Description
【背景】
在某学期，有n门课程需要学习，编号为：0 ~ n - 1。
某些课程有依赖关系，例如：学习C++前必须先把C语言学习了。
如果course 0学习前必须要先学习course 1的话，记为：[0, 1]。
【输入】
给定数字n代表课程数目。
给定二维数组prerequisite代表所有的依赖关系。
【输出】
判断能否把所有的课程学习完成。
若可以，给出一个可能的顺序（可能的顺序可能不止一个，给出一个即可）。

### 2.Example
Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both 
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. 
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .

### 3.Corner Case
1. 考虑n的合法性。
2. 考虑cntEdges的合法性。

## 二、题解
### 分析
参考Leetcode-207
### Solution-1：Topological Sort(Kahn's Algorithm)(BFS)
1.出队列的顺序即为一个有效的拓扑顺序。
2.在return的时候判断ans中是否包含了所有的点即可。否则，说明有环，返回空数组。

时间复杂度：O(n + V) //n是节点个数，V是边的条数
空间复杂度：O(n ^ 2)

3.Code(https://leetcode.com/submissions/detail/287934339/)
```C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> ans;
        int cntEdges = prerequisites.size();
        vector<vector<int>> graph(numCourses);
        vector<int> indegree(numCourses, 0);
        for (int i = 0; i <= cntEdges - 1; i++) {
            //edge: u -> v
            int u = prerequisites[i][1];
            int v = prerequisites[i][0];
            graph[u].push_back(v);
            indegree[v]++;
        }
        queue<int> q;
        for (int i = 0; i <= numCourses - 1; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        while (!q.empty()) {
            int temp = q.front();
            q.pop();
            ans.push_back(temp);
            for (int v : graph[temp]) {
                if (--indegree[v] == 0) {
                    q.push(v);
                }
            }
        }
        if (ans.size() != numCourses) {
            ans.clear();
        }
        return ans;
    }
};
```

