---
description: Microsoft
---

# Merge Sorted Array

[64. Merge Sorted Array](https://www.lintcode.com/problem/merge-sorted-array/?_from=ladder&&fromId=115)

#### 題目

Given two sorted integer arrays A and B, merge B into A as one sorted array.

Example Example 1:

```text
Input：[1, 2, 3] 3  [4,5]  2
Output：[1,2,3,4,5]
Explanation:
After merge, A will be filled as [1, 2, 3, 4, 5]
```

You may assume that A has enough space \(size that is greater or equal to m + n\) to hold additional elements from B. The number of elements initialized in A and B are m and n respectively.

#### 解法

```cpp
class Solution {
public:
    /*
     * @param A: sorted integer array A which has m elements, but size of A is m+n
     * @param m: An integer
     * @param B: sorted integer array B which has n elements
     * @param n: An integer
     * @return: nothing
     */
    void mergeSortedArray(int A[], int m, int B[], int n) {
        // write your code here
        int aIndex = m - 1;
        int bIndex = n - 1;
        int pos = m + n - 1;

        while(aIndex >= 0 && bIndex >= 0){
            A[pos--] = (A[aIndex] > B[bIndex]) ? A[aIndex--] : B[bIndex--];
        }

        while(aIndex >= 0){
            A[pos--] = A[aIndex--];
        }

        while(bIndex >= 0){
            A[pos--] = B[bIndex--];
        }
    }
};
```

