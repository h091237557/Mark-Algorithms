---
description: 注意、Amazon
---

# Climbing Stairs II

[272. Climbing Stairs II](https://www.lintcode.com/problem/climbing-stairs-ii/description?_from=ladder&&fromId=59)

#### 題目

A child is running up a staircase with n steps, and can hop either 1 step, 2 steps, or 3 steps at a time. Implement a method to count how many possible ways the child can run up the stairs.

Example Example 1:

```text
Input: 3
Output: 4
Explanation: 1 + 1 + 1 = 2 + 1 = 1 + 2 = 3 = 3 , there are 4 ways.
```

Example 2:

```text
Input: 4
Output: 7
Explanation: 1 + 1 + 1 + 1 = 1 + 1 + 2 = 1 + 2 + 1 = 2 + 1 + 1 = 2 + 2 = 1 + 3 = 3 + 1 = 4 , there are 7 ways.
```

Clarification For n=0, we think the answer is 1.

#### 解法 - dfs

```cpp
class Solution {
private:
    int count = 0;
    void dfsHelper(int target){
        if(target == 0){
            count++;
            return;
        }
        if(target < 0){
            return;
        }

        for (int i=1; i <= 3; i++){
            dfsHelper(target - i);
        }
    }
public:
    /**
     * @param n: An integer
     * @return: An Integer
     */
    int climbStairs2(int n) {
        // write your code here
        dfsHelper(n);
        return count;
    }
};
```

#### 解法 - DP

```text
dp[i] => 第 i 階有幾種方法

dp[i] = dp[i-1] + dp[i-2] + dp[i-3]
```

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: An Integer
     */
    int climbStairs2(int n) {
        // write your code here
        vector<int> dp(n+1);
        dp[0] = 1;
        dp[1] = 1;
        dp[2] = 2;


        for (int i=3; i <= n; i++){
            dp[i] = dp[i-1] + dp[i-2] + dp[i-3];
        }
        return dp[n];
    }
};
```

