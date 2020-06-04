# Can Reach The Endpoint

[1479. Can Reach The Endpoint](https://www.lintcode.com/problem/can-reach-the-endpoint/?_from=ladder&&fromId=59)

#### 題目

Given a map size of m\*n, 1 means space, 0 means obstacle, 9 means the endpoint. You start at \(0,0\) and return whether you can reach the endpoint.

Example Example1

```text
Input: 
[
    [1,1,1],
    [1,1,1],
    [1,1,9]
]
Output: true
```

Example2

```text
Input: 
[
    [1,1,1],
    [1,0,0],
    [1,0,9]
]
Output: false
```

#### 解法 - DFS

```cpp
class Solution {
private:
    bool isFind =false;
    void bfsHelper(vector<vector<int>> &map, int row, int col){

        if(isBound(map, row, col)){
            return;
        }

        if(map[row][col] == 9){
            isFind = true;
            return;
        }

        map[row][col] = 0;
        bfsHelper(map, row-1, col);
        bfsHelper(map, row+1, col);
        bfsHelper(map, row, col-1);
        bfsHelper(map, row, col+1);
    }
    bool isBound(vector<vector<int>> &map, int row, int col){
        if(row >= map.size() || row < 0){
            return true;
        }
        if(col >= map[0].size() || col < 0){
            return true;
        }
        if(map[row][col] == 0){
            return true;
        }

        return false;
    }
public:
    /**
     * @param map: the map
     * @return: can you reach the endpoint
     */
    bool reachEndpoint(vector<vector<int>> &map) {
        bfsHelper(map, 0, 0);
        return isFind;
    }
};
```

