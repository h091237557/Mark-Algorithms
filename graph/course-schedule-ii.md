---
description: July30
---

# Course Schedule II

[210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

### 題目

There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: \[0,1\]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

Example 1:

```text
Input: 2, [[1,0]] 
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished   
             course 0. So the correct course order is [0,1] .
```

Example 2:

```text
Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both     
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. 
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
```

Note:

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites

### 解法思路 - 拓樸排序

就是標準拓樸排序流程。

1. 建立每個課程的 degree 表，也就是一個課程有幾門先修課
2. 建立修課圖，也就是一個課程修完後，可以修什麼課程。
3. 使用 BFS 開始修課，每一次修課要更新每門課的 degree 值。
4. degree 為 0 的值才可以修。

需要注意幾個 corner case :

* 有 n 門課程，但是沒有先修課的情境。
* 有先修課，但出現 \[1,0\]\[0,1\] 這種情境。

```text
Time complexity : O(V+E)
Space complexity : O(V+E)
```

```cpp
class Solution {
private:
    // def: index => course, value => degree 
    vector<int> degree_map;
    // def: map<course, next courses>
    unordered_map<int, vector<int>> course_map;
    void generateMap(int numCourses, vector<vector<int>>& prerequisites){
        degree_map = vector<int>(numCourses, 0);

        for (int i=0; i < prerequisites.size(); i++){
            int course = prerequisites[i][0];
            int pre_course = prerequisites[i][1];
            degree_map[course]++;
            course_map[pre_course].push_back(course);
        }
    }    
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> res;
        if(numCourses == 0){
            return res;
        }

        generateMap(numCourses, prerequisites);
        std::queue<int> queue;

        // 將先修課為 0 的 course 丟到 queue 中
        for (int i=0; i < numCourses; i++){
            if(degree_map[i] == 0){
                queue.push(i);
            }
        }

        while(!queue.empty()){
            int size = queue.size();
            while(size != 0){
                size--;
                int cur_course = queue.front();
                res.push_back(cur_course);
                queue.pop();
                numCourses--;

                vector<int> nexts = course_map[cur_course];

                for (int i=0; i < nexts.size(); i++){
                    degree_map[nexts[i]]--;
                    if(degree_map[nexts[i]] == 0){
                        queue.push(nexts[i]);
                    }
                }
            }
        }
        if(numCourses == 0){
            return res; 
        }

        return vector<int>(0);
    }
};
```

