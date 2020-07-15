---
description: contest197
---

# Path with Maximum Probability

[1514. Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

### 題目

You are given an undirected weighted graph of n nodes \(0-indexed\), represented by an edge list where edges\[i\] = \[a, b\] is an undirected edge connecting the nodes a and b with a probability of success of traversing that edge succProb\[i\].

Given two nodes start and end, find the path with the maximum probability of success to go from start to end and return its success probability.

If there is no path from start to end, return 0. Your answer will be accepted if it differs from the correct answer by at most 1e-5.

Example 1:

![](https://assets.leetcode.com/uploads/2019/09/20/1558_ex1.png)

```text
Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
Output: 0.25000
Explanation: There are two paths from start to end, one having a probability of success = 0.2 and the other has 0.5 * 0.5 = 0.25.
```

Example 2:

![](https://assets.leetcode.com/uploads/2019/09/20/1558_ex2.png)

```text
Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.3], start = 0, end = 2
Output: 0.30000
```

Example 3:

![](https://assets.leetcode.com/uploads/2019/09/20/1558_ex3.png)

```text
Input: n = 3, edges = [[0,1]], succProb = [0.5], start = 0, end = 2
Output: 0.00000
Explanation: There is no path between 0 and 2.
```

Constraints:

* 2 &lt;= n &lt;= 10^4
* 0 &lt;= start, end &lt; n
* start != end
* 0 &lt;= a, b &lt; n
* a != b
* 0 &lt;= succProb.length == edges.length &lt;= 2\*10^4
* 0 &lt;= succProb\[i\] &lt;= 1
* There is at most one edge between every two nodes.

### 解法思路 - DFS \( Time limit \)

```cpp
class Solution {
private:
    // def: map<節點,<下一個節點,機率>>
    double res;
    unordered_map<int, vector<pair<int, double>>> graph;
    void generateGraph(int n,vector<vector<int>>& edges, vector<double>& succProb){
        for (int i=0; i < n; i++){
            vector<pair<int,double>> nexts;
            for (int j=0; j < edges.size(); j++){
                if(edges[j][0] == i){
                    nexts.push_back({edges[j][1], succProb[j]});
                }
                if(edges[j][1] == i){
                    nexts.push_back({edges[j][0], succProb[j]});
                }
            }
            graph[i] = nexts;
        }
    };
    void dfsHelper(int cur, int end, double p, unordered_set<int> visited, int n){
        if(p < res){
            return;
        }

        if(cur == end){
            res = max(res, p);
            return;
        }
        if(visited.size() == n){
            return;
        }

        vector<pair<int, double>> nexts = graph[cur];
        for (int i=0; i < nexts.size(); i++){
            int next = nexts[i].first;
            if(visited.count(next)) continue;
            double next_p = p*nexts[i].second;

            visited.insert(next);
            dfsHelper(next, end, next_p, visited, n);
            visited.erase(next);
        }
    }
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {

        generateGraph(n, edges, succProb);
        unordered_set<int> visited;
        dfsHelper(start, end, 1, visited, n);

        return res;
    }
};
```

```text
https://leetcode.com/submissions/detail/365536495/testcase/
```

### 解法思路 - Dijkstra's algorithm

[戴克斯特拉演算法](http://nthucad.cs.nthu.edu.tw/~yyliu/personal/nou/04ds/dijkstra.html)

> 注意他無法處理權重為『 負 』的情況，解法為 Bellman-Ford 演算法

```text
Time complexity : O(V + LogV)
Space complexity : O(V + E)
```

```cpp
class Solution {
private:
    unordered_map<int, vector<pair<int, double>>> graph;
    void generateGraph(vector<vector<int>>& edges, vector<double>& succProb){
        for (int i=0; i < edges.size(); i++){
            graph[edges[i][0]].push_back({ edges[i][1], succProb[i]});
            graph[edges[i][1]].push_back({ edges[i][0], succProb[i]});
        }
    }
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        generateGraph(edges, succProb);
        std::queue<pair<int, double>> queue;
        queue.push({start, 1});
        vector<double> shortProb(n, 0);
        shortProb[start] = 1;

        while(!queue.empty()){
            int size = queue.size();
            while(size != 0){
                size--;
                pair<int, double> cur = queue.front();
                queue.pop();
                int node = cur.first;
                double p = cur.second;

                vector<pair<int, double>> nexts = graph[node];

                for (int i=0; i < nexts.size(); i++){
                    int next_node = nexts[i].first;
                    double next_p = nexts[i].second * p;
                    if(next_p <= shortProb[next_node]) continue;

                    shortProb[next_node] = next_p;
                    queue.push({next_node, next_p});
                }
            }
        }
        return shortProb[end];
    }
};
```

### 解法思路 - Bellman Ford

```text
Time complexity: O(V*E)
Space complexity: O(V+E)
```

```cpp
class Solution {
private:
    unordered_map<int, vector<pair<int, double>>> graph;
    void generateGraph(vector<vector<int>>& edges, vector<double>& succProb){
        for (int i=0; i < edges.size(); i++){
            graph[edges[i][0]].push_back({ edges[i][1], succProb[i]});
            graph[edges[i][1]].push_back({ edges[i][0], succProb[i]});
        }
    }
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        generateGraph(edges, succProb);
        std::queue<pair<int, double>> queue;
        queue.push({start, 1});
        vector<double> shortProb(n, 0);
        shortProb[start] = 1;
        unordered_set<int> visited;

        while(!queue.empty()){
            int size = queue.size();
            while(size != 0){
                size--;
                pair<int, double> cur = queue.front();
                queue.pop();
                int node = cur.first;
                double p = cur.second;

                visited.insert(node);
                vector<pair<int, double>> nexts = graph[node];

                for (int i=0; i < nexts.size(); i++){
                    int next_node = nexts[i].first;
                    double next_p = nexts[i].second * p;
                    if(next_p <= shortProb[next_node]) continue;

                    shortProb[next_node] = next_p;
                    queue.push({next_node, next_p});
                }
            }
        }
        return shortProb[end];
    }
};
```

