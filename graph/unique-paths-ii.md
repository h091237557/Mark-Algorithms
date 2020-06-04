---
description: 注意、Amazon
---

# Unique Paths II

[115. Unique Paths II](https://www.lintcode.com/problem/unique-paths-ii/?_from=ladder&&fromId=119)

#### 題目

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

Example

```text
Example 1:
    Input: [[0]]
    Output: 1


Example 2:
    Input:  [[0,0,0],[0,1,0],[0,0,0]]
    Output: 2

    Explanation:
    Only 2 different path.
```

#### 解法 - DP \(二維\)

```cpp
class Solution {
public:
    /**
     * @param obstacleGrid: A list of lists of integers
     * @return: An integer
     */
    int uniquePathsWithObstacles(vector<vector<int>> &grid) {
        // write your code here

        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int row = 0; row < m; row++){
            for (int col = 0; col < n; col++){
                if(grid[row][col] == 1){
                    dp[row][col] = 0;
                    continue;
                }

                if(row == 0 && col == 0){
                    dp[row][col] = 1;
                    continue;
                }

                if(row > 0){
                    dp[row][col] += dp[row-1][col];
                }

                if(col > 0){
                    dp[row][col] += dp[row][col-1];
                }
            }
        }

        return dp[m-1][n-1];
    }
};
```

#### 解法 - DP \( 1 維 \)

```cpp
class Solution {
public:
    /**
     * @param obstacleGrid: A list of lists of integers
     * @return: An integer
     */
    int uniquePathsWithObstacles(vector<vector<int>> &grid) {
        // write your code here
        int m = grid.size();
        int n = grid[0].size();
        // def: res[i] 代表走到最底層的 i column 有幾種方法 
        vector<int> dp(m, 0);
        dp[0] = 1;

        for (int row = 0; row < m; row++){
            if(grid[row][0] == 1){
                dp[0] = 0;
            }

            for (int col = 0; col < n; col++){
                if(grid[row][col] == 1){
                    dp[col] = 0;
                }else{
                    dp[col] += dp[col-1];
                }
            }
        }
        return dp[n-1];
    }
};
```

