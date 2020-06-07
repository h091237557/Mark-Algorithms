# Coin Change 2

[518. Coin Change 2](https://leetcode.com/problems/coin-change-2/)

#### 題目

You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

```text
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.

Input: amount = 10, coins = [10] 
Output: 1
```

Note:

You can assume that

* 0 &lt;= amount &lt;= 5000
* 1 &lt;= coin &lt;= 5000
* the number of coins is less than 500
* the answer is guaranteed to fit into signed 32-bit integer

#### 解法 - DP

這題 dp 的思路比較像是，先只用第 1 個硬幣計算 coins\[i\] 至 amount 的方法。

```text
Ex. Amount = 5 Coins = [1,2,5]
dp[0] = 1

裡面的 for j loop

dp[1] = 1 => 1
dp[2] = 1 => 1,1
dp[3] = 1 => 1,1,1
dp[4] = 1 => 1,1,1,1
dp[5] = 1 => 1,1,1,1,1

上述代表 1 ~ amount 只用 1 coin 可以拼出的方法，每一個值都是 1。

dp[2] = 2 => 1,1 與 2
dp[3] = 2 => 1,1,1 與 1,2
dp[4] = 1 + 2 = 3 => 1,1,1,1 與 1,1,2 與 2,2
```

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        // def: dp[i] => 可以使用 coins 拼出 i 的方法數
        vector<int> dp(amount+1, 0);
        dp[0] = 1;

        for (int i=0; i < coins.size(); i++){
            for (int j=coins[i]; j <= amount; j++){
                dp[j] += dp[j - coins[i]];
            }
        }

        return dp[amount];
    }
};
```

