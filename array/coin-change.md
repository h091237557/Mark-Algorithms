# Coin Change

[669. Coin Change](https://www.lintcode.com/problem/coin-change/description)

#### 題目

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example Example1

```text
Input: 
[1, 2, 5]
11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

```text
Input: 
[2]
3
Output: -1
```

#### 解法 - DP

```cpp
class Solution {
public:
    /**
     * @param coins: a list of integer
     * @param amount: a total amount of money amount
     * @return: the fewest number of coins that you need to make up
     */
    int coinChange(vector<int> &coins, int amount) {
        // write your code here
        // dp[amount] : 可拼成 amount 的最少 coin 量
        // dp[amount] = min(dp[amount-coins[i]], dp[amount-coint[i+1]]..)
        if(coins.size() == 0){
            return -1;
        }

        vector<int> dp(amount+1, INT_MAX);
        dp[0] = 0;

        for (int a = 1; a <= amount; a++){
            for (int i=0; i < coins.size(); i++){
                if(a < coins[i] || dp[a- coins[i]] == INT_MAX) continue;
                dp[a] = min(dp[a], dp[a - coins[i]] + 1) ;
            }
        }
        if(dp[amount] == INT_MAX){
            return -1;
        }
        return dp[amount];
    }
};
```

