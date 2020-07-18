# Course Schedule

[615. Course Schedule](https://www.lintcode.com/problem/course-schedule/description)

### 題目

There are a total of n courses you have to take, labeled from 0 to n - 1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: \[0,1\]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

Example Example 1:

```text
Input: n = 2, prerequisites = [[1,0]] 
Output: true
```

Example 2:

```text
Input: n = 2, prerequisites = [[1,0],[0,1]] 
Output: false
```

### 解法 - 拓排 \( 比較慢，但比較清楚 \)

```cpp
class Solution {
private:
    unordered_map<int, vector<int>> generateCourseMap(int n, vector<pair<int,int>>& prerequisites){
        // def: <course , next Course>
        unordered_map<int,vector<int>> res;

        for (int i=0; i < n; i++){
            vector<int> temp;
            res[i] = temp;
        }

        for (int j=0; j < prerequisites.size(); j++){
            int course = prerequisites[j].first;
            int prev_course = prerequisites[j].second;
            res[prev_course].push_back(course);
        }

        return res;
    }
    unordered_map<int, int> generateDegree(vector<pair<int,int>>& prerequisites){
        // def: <course, degree count>
        unordered_map<int,int> res;

        for (int i=0; i < prerequisites.size(); i++){
            int prev_course = prerequisites[i].first;

            if(res.count(prev_course)){
                res[prev_course]++;
            }else{
                res[prev_course] = 1;
            }
        }

        return res;
    }
public:
    /*
     * @param numCourses: a total of n courses
     * @param prerequisites: a list of prerequisite pairs
     * @return: true if can finish all courses or false
     */
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        // write your code here
        if(prerequisites.size() == 0 || numCourses == 0){
            return true;
        }
        int n = numCourses;

        unordered_map<int, vector<int>> course_map = generateCourseMap(n,prerequisites);
        unordered_map<int, int> degree = generateDegree(prerequisites);
        std::queue<int> queue;

        for (int i=0; i < n; i++){
            if(!degree.count(i)){
                queue.push(i);
            }
        }

        int required_course_num = n;
        while(!queue.empty()){
            int course = queue.front();
            queue.pop();
            required_course_num--;

            vector<int> next_courses = course_map[course];

            for (int i=0; i < next_courses.size(); i++){
                degree[next_courses[i]]--;

                if(degree[next_courses[i]] == 0){
                    queue.push(next_courses[i]);
                }
            }
        }

        return required_course_num == 0;
    }
};
```

### 解法 - 拓排 \( 比較快，兩個 map 合在一起產生 \)

```cpp
class Solution {
public:
    /*
     * @param numCourses: a total of n courses
     * @param prerequisites: a list of prerequisite pairs
     * @return: true if can finish all courses or false
     */
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        // write your code here
        if(prerequisites.size() == 0 || numCourses == 0){
            return true;
        }

        // tip : map 與 degree 可以一起產生
        // step 1. create course map and degree map
        unordered_map<int, vector<int>> course_map;
        unordered_map<int, int> degree;

        for (int i=0; i < numCourses; i++){
            vector<int> temp;
            course_map[i] = temp;
        }

        for (int j=0; j < prerequisites.size(); j++){
            int course = prerequisites[j].first;
            int prev_course = prerequisites[j].second;

            course_map[prev_course].push_back(course);

            if(degree.count(course)){
                degree[course]++;
            }else{
                degree[course] = 1;
            }
        }

        // step 2. find start
        std::queue<int> queue;
        for (int i=0; i < numCourses; i++){
            if(!degree.count(i)){
                queue.push(i);
            }
        }

        int required_course_num = numCourses;
        while(!queue.empty()){
            int course = queue.front();
            queue.pop();
            required_course_num--;

            vector<int> next_courses = course_map[course];

            for (int i=0; i < next_courses.size(); i++){
                degree[next_courses[i]]--;

                if(degree[next_courses[i]] == 0){
                    queue.push(next_courses[i]);
                }
            }
        }

        return required_course_num == 0;
    }
};
```

