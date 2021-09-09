---
title: 207.Course Schedule
---

## 一、题意
题目链接: [207.Course Schedule](https://leetcode.com/problems/course-schedule/)
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

### 2.Example
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

### 3.Corner Case
1. 判断n的合法性
2. 判断cntEdges的合法性

## 二、题解
### 分析
本题目实际上是：求一张有向图中是否有环。
n代表：该有向图中有n个节点。
prerequisite代表：该有向图中所有的边。
### Solution-1：Topological Sort(Kahn's Algorithm)(BFS)
1.Kahn's Algorithm本质上是一个基于入度的拓扑排序。
具体步骤是，我们每次去掉入度为0的点，将该点加入拓扑排序，
同时删去与其连接的边（其它节点的入度会受到影响），
直到去掉所有的点为止，
如果中途遇到不存在入度为0的点的情况，那么，就认为这个有向图不是拓扑排序的。

2.具体过程如下：
首先，根据输入，把图存起来，此处使用二维vector作为邻接表。（还可以使用二维vector作为邻接矩阵）。
同时，存储图的时候，计算每个节点的入度indegree。
然后，将图中所有入度为0的点依次进入队列。
最后，每次从队首拿出一个元素来，将其放入拓扑排序的结果中。将其入度置为-1。
    同时，遍历其后继节点，将它们的入度都减一。如果减一后为0，则继续入队。
当所有点都考虑完成后，拓扑排序的结果应该包含所有点。若不包含某些点，说明该有向图中有环存在。

时间复杂度：O(n + V) //n是节点个数，V是边的条数
空间复杂度：O(n ^ 2)

3.Code(https://leetcode.com/submissions/detail/287858657/)
```C++
class Solution {

public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> indegree(numCourses, 0);
        int cntEdge = prerequisites.size();
        for (int i = 0; i <= cntEdge - 1; i++) {
            int u = prerequisites[i][1];
            int v = prerequisites[i][0];
            graph[u].push_back(v);
            indegree[v]++;
        }
        
        queue<int> q;
        vector<int> topologicalRes;
        for (int i = 0; i <= numCourses - 1; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        while (!q.empty()) {
            int temp = q.front();
            q.pop();
            topologicalRes.push_back(temp);
            numCourses--;
            for (int v : graph[temp]) {
                if (--indegree[v] == 0) {
                    q.push(v);
                }
            }
        }
        // for (int x : topologicalRes) {
        //     cout << x << " ";
        // }
        // cout << endl;
        return numCourses == 0;
    }
};
```

### Solution-2：DFS
1.采用DFS的路径，从每个节点一直往下走，将走过的节点标记。
若中途又经过了本节点，说明一定存在环。

2.具体过程如下：
首先，先将图存起来，这次我们用邻接矩阵存储。
同时，开一个大小为n的数组，用于标记每一个节点。
然后，从第一个节点开始进行dfs，依次考察所有n个节点。若某个节点处发现了环，则直接return false。
在某个节点i处dfs过程：
首先判断节点i在之前是否已经判断通过了，如果通过了，则return true。
其次判断节点i是否在本轮中正在考察，如果正在考察的同时，又遍历到了自己，说明一定有环。
最后：
a.将visited[i] = 1;
b.以其所有后继节点为起点，依次考察。
c.如果都通过了，则将visited[i] = -1;代表以i为起点的dfs路径，没有发现环。return true即可。

时间复杂度：O(n + V)
空间复杂度：O(n ^ 2)

3.Code(https://leetcode.com/submissions/detail/287867955/)
```C++
class Solution {
private:
    bool dfs(int i, vector<vector<int>>& graph, vector<int>& visited) {
        int n = graph.size();
        if (visited[i] == 1) {
            return false;
        }
        if (visited[i] == -1) {
            return true;
        }
        visited[i] = 1;
        for (int j = 0; j <= n - 1; j++) {
            if (graph[i][j] == 1 && dfs(j, graph, visited) == false) {
                return false;
            }
        }
        visited[i] = -1;
        return true;
    }
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses, vector<int>(numCourses, 0));
        vector<int> visited(numCourses, 0);
        int cntEdge = prerequisites.size();
        for (int i = 0; i <= cntEdge - 1; i++) {
            int u = prerequisites[i][1];
            int v = prerequisites[i][0];
            graph[u][v] = 1;
        }
        
        for (int i = 0; i <= numCourses - 1; i++) {
            if (dfs(i, graph, visited) == false) {
                return false;
            }
        }
        return true;
    }
};
```





























