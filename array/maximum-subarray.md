---
description: 注意、Microsoft
---

# Maximum Subarray

[41. Maximum Subarray](https://www.lintcode.com/problem/maximum-subarray/?_from=ladder&&fromId=88)

#### 題目

Given an array of integers, find a contiguous subarray which has the largest sum.

Example Example1:

```text
Input: [−2,2,−3,4,−1,2,1,−5,3]
Output: 6
Explanation: the contiguous subarray [4,−1,2,1] has the largest sum = 6.
```

Example2:

```text
Input: [1,2,3,4]
Output: 10
Explanation: the contiguous subarray [1,2,3,4] has the largest sum = 10.
```

Challenge Can you do it in time complexity O\(n\)?

#### 解法 - DP

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: An integer indicate the sum of max subarray
     */
    int maxSubArray(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        if(n == 0){
            return 0;
        }

        // def: 到 maxSubArray(0...i) dp[i]
        // dp[i] = (dp[i-1] < 0) ? nums[i] : dp[i-1] + nums[i]
        vector<int> dp(n, 0);
        int maxNum = nums[0];
        dp[0] = nums[0];

        for (int i=1; i < n; i++){
            if(dp[i-1] > 0){
                dp[i] = dp[i-1] + nums[i];
            }else{
                dp[i] = nums[i];
            }
            maxNum = max(maxNum, dp[i]);
        }
        return maxNum;
    }
};
```

#### 解法 - Greedy

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: An integer indicate the sum of max subarray
     */
    int maxSubArray(vector<int> &nums) {
        // write your code here
        int maxSum = INT_MIN;
        int sum = 0;

        for (int i=0; i < nums.size(); i++){
            sum += nums[i];
            maxSum = max(maxSum, sum);
            sum = max(0, sum);
        }
        return maxSum;
    }
};
```

