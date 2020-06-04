# Number of Islands

[433. Number of Islands](https://www.lintcode.com/problem/number-of-islands/description?_from=ladder&&fromId=15)

#### 題目

Given a boolean 2D matrix, 0 is represented as the sea, 1 is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.

Find the number of islands.

Example Example 1:

```text
Input:
[
  [1,1,0,0,0],
  [0,1,0,0,1],
  [0,0,0,1,1],
  [0,0,0,0,0],
  [0,0,0,0,1]
]
Output:
3
```

Example 2:

```text
Input:
[
  [1,1]
]
Output:
1
```

#### 解法 - DFS

流程如下 :

1. 尋找到 1 的點當起點。
2. 進行 bfs 走動，且將走過的格子設為 0。
3. 走完後，再從 step 1 開始。
4. 計算走過 step1 ~ step 3 的次數就是答案。

```cpp
// DFS
class Solution {
private:
    void dfsHelper(vector<vector<bool>>& grid, int row, int col){

        if(isBound(row, col, grid)){
            return;
        }

        grid[row][col] = false;
        // up, down, left, right
        dfsHelper(grid, row-1, col);
        dfsHelper(grid, row+1, col);
        dfsHelper(grid, row, col-1);
        dfsHelper(grid, row, col+1);
    }
    bool isBound(int row, int col, vector<vector<bool>>& grid){

        if(row >= grid.size() || row < 0){
            return true;
        }

        if(col >= grid[0].size() || col < 0){
            return true;
        }

        if(grid[row][col] == false){
            return true;
        }

        return false;
    }
public:
    /**
     * @param grid: a boolean 2D matrix
     * @return: an integer
     */
    int numIslands(vector<vector<bool>> &grid) {
        // write your code here

        int res = 0;
        for (int row=0; row < grid.size(); row++){
            for (int col=0; col < grid[0].size(); col++){
                if(grid[row][col] == true){
                    res++;
                    dfsHelper(grid, row, col);
                }
            }
        }
        return res;
    }
};
```

#### 解法 - BFS

```cpp
class Solution {
private:
    void bfsHelper(
        vector<vector<bool>> &grid,
        int row,
        int col
    ){
        queue<pair<int,int>> que;
        que.push(make_pair(row, col));

        while(!que.empty()){
            pair<int,int> cur = que.front();
            que.pop();
            if(isBound(grid, cur.first, cur.second)){
                continue;
            }
            grid[cur.first][cur.second] = false;

            que.push(make_pair(cur.first - 1, cur.second));
            que.push(make_pair(cur.first + 1, cur.second));
            que.push(make_pair(cur.first, cur.second - 1));
            que.push(make_pair(cur.first, cur.second + 1));
        }
    }
    bool isBound(vector<vector<bool>> &grid, int row, int col){
        if(row >= grid.size() || row < 0){
            return true;
        }

        if(col >= grid[0].size() || col < 0 ){
            return true;
        }

        if(grid[row][col] == false){
            return true;
        }

        return false;
    }
public:
    /**
     * @param grid: a boolean 2D matrix
     * @return: an integer
     */
    int numIslands(vector<vector<bool>> &grid) {
        // write your code here
        int res = 0;
        for (int row=0; row < grid.size(); row++){
            for (int col=0; col < grid[0].size(); col++){
                if(grid[row][col] == true){
                    bfsHelper(grid,row,col);
                    res++;
                }
            }
        }
        return res;
    }
};
```

