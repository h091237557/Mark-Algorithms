---
description: Facebook、Google
---

# Palindromic Substrings

[837. Palindromic Substrings](https://www.lintcode.com/problem/palindromic-substrings/?_from=ladder&&fromId=130)

#### 題目

Given a string, your task is to count how many palindromic substrings in this string. The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

Example Example1

```text
Input: "abc"
Output: 3
Explanation:
3 palindromic strings: "a", "b", "c".
```

```text
Input: "aba"
Output: 4
Explanation:
4 palindromic strings: "a", "b", "a", "aba".
```

Notice The input string length won't exceed 1000

#### 解法 - DP

**Step 1. 確認狀態**

```text
dp[l][r] => s[l...r] 是否為 Palindromic。
```

**Step 2. 轉移方程式**

```text
dp[l][r] = 
   if(s[l] == s[r] && ( r - l <= 2 )) = true
   if(s[l] == s[r] && dp[l+1][r-1] = true
```

r - 1 &gt;= 1 的意思為，只有在 aba or abbb 也就是三個字元以上的，才能處理。

**Step 3. 初始條件**

```text
dp[0][0]
...
dp[n-1][n-1]
= true
```

**Step 4. 計算順序**

```text
N = 3
00 -> 11 -> 22 -> 01 -> 12 -> 012
```

**程式碼**

```cpp
class Solution {
public:
    /**
     * @param str: s string
     * @return: return an integer, denote the number of the palindromic substrings
     */
    int countPalindromicSubstrings(string &str) {
        // write your code here
        int size = str.size();
        int res = 0;
        vector<vector<bool>> dp(size, vector<bool>(size, false));

        for (int l=(size-1); l >=0 ; l--){
            for (int r=l; r < size; r++){

                if(str[l] == str[r] && (r - l <= 2 || dp[l+1][r-1])){
                    dp[l][r] = true;
                }

                if(dp[l][r]){
                    res++;
                }
            }
        }

        return res;
    }
};
```

