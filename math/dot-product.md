---
description: Amazon、Microsoft
---

# Dot Product

[1480. Dot Product](https://www.lintcode.com/problem/dot-product/?_from=ladder&&fromId=59)

[dot\_product](https://en.wikipedia.org/wiki/Dot_product)

#### 題目

Given two array, output their dot product

Example Example1

```text
Input: A = [1,1,1] and B = [2,2,2]
Output: 6
Explanation:
1*2+1*2+1*2=6
```

Example2

```text
Input: A = [3,2] and B = [2,3,3]
Output: -1
Explanation:
there is no dot product
```

#### 解法

```cpp
class Solution {
public:
    /**
     * @param A: an array
     * @param B: an array
     * @return: dot product of two array
     */
    int dotProduct(vector<int> &A, vector<int> &B) {
        // Write your code here
        if(A.size() == 0 || B.size() == 0){
            return -1;
        }

        if(A.size() != B.size()){
            return -1;
        }

        int res = 0;
        for (int i=0; i < A.size(); i++){
            res += ( A[i] * B[i] );
        }
        return res;
    }
};
```

