---
description: Contest 192
---

# Shuffle the Array

[5428. Shuffle the Array](https://leetcode.com/problems/shuffle-the-array/)

#### 題目

Given the array nums consisting of 2n elements in the form \[x1,x2,...,xn,y1,y2,...,yn\].

Return the array in the form \[x1,y1,x2,y2,...,xn,yn\].

```text
Input: nums = [2,5,1,3,4,7], n = 3
Output: [2,3,5,4,1,7] 
Explanation: Since x1=2, x2=5, x3=1, y1=3, y2=4, y3=7 then the answer is [2,3,5,4,1,7].
```

```text
nput: nums = [1,2,3,4,4,3,2,1], n = 4
Output: [1,4,2,3,3,2,4,1]
```

```text
Input: nums = [1,1,2,2], n = 2
Output: [1,2,1,2]
```

Constraints:

* 1 &lt;= n &lt;= 500 
* nums.length == 2n 
* 1 &lt;= nums\[i\] &lt;= 10^3

#### 解法 - New Vector

```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        vector<int> res;
        int left = 0;
        int right = n;
        while(left < n || right < nums.size()){
            if(left < n){
                res.push_back(nums[left]);
                left++;
            }
            if(right < nums.size()){
                res.push_back(nums[right]);
                right++;
            }
        }
        return res;
    }
};
```

