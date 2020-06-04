---
description: Amazon
---

# Search a 2D Matrix

[28. Search a 2D Matrix](https://www.lintcode.com/problem/search-a-2d-matrix/?_from=ladder&&fromId=133)

#### 題目

Write an efficient algorithm that searches for a value in an m x n matrix.

This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

Example

```text
Example 1:
    Input:  [[5]],2
    Output: false

    Explanation: 
    false if not included.

Example 2:
    Input:  [
    [1, 3, 5, 7],
    [10, 11, 16, 20],
    [23, 30, 34, 50]
],3
    Output: true

    Explanation: 
    return true if included.
```

Challenge O\(log\(n\) + log\(m\)\) time

#### 解法 - Binary search

```cpp
class Solution {
public:
    /**
     * @param matrix: matrix, a list of lists of integers
     * @param target: An integer
     * @return: a boolean, indicate whether matrix contains target
     */
    bool searchMatrix(vector<vector<int>> &matrix, int target) {
        // write your code here
        int start = 0;
        int m = matrix.size();
        int n = matrix[0].size();
        int end = m * n - 1; 

        // why plus one ?
        while(start + 1 < end){
            int mid = start + (end - start) / 2;
            int mid_row = mid / n;
            int mid_col = mid % n;

            if(matrix[mid_row][mid_col] < target){
                start = mid;
            }else{
                end = mid;
            }
        }

        if(matrix[start/n][start % n] == target){
            return true;
        }
        if(matrix[end/n][end % n] == target){
            return true;
        }
        return false;
    }
};
```

