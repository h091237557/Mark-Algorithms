---
description: 注意、Microsoft
---

# Minimum Moves

[1822. Minimum Moves](https://www.lintcode.com/problem/minimum-moves/?_from=ladder&&fromId=143)

#### 題目

Given a string S consisting of N letters 'a' and / or 'b'. In one move, you can swap one letter for the other \('a' for 'b' or 'b' for 'a'\). You should return the minimum number of moves required to obtain a string containing no instances of three identical consecutive letters.

Example

```text
Example1:
Input:
S = "baaaaa"
Output: 1
Explanation: The string S without three identical consecutive letters which can be obtained in one move is "baabaa".
Example2:
Input:
S = "baaabbaabbba"
Output: 2
Explanation: There are four valid strings obtainable in two moves: for example: "bbaabbaabbaa"
Example3:
Input:
S="baabab"
Output: 0
```

Notice

* N is an integer within the range: \[0, 2000000\]
* string S consists only of the characters 'a' and/or 'b'

#### 解法

這題是問你說給定一個只有 a 或 b 的字串 S，問最多要 swap 幾次，才能保證 S 中沒有『 連續三個相同的字母 』，例如 S = "baaaaa" 需要 swap 一次，才能確保沒有連續三個 a 。

這一題我們不用計算出『 那兩個位置需要 swap 』，那這樣我們可以改用以下的想法來解。

> 計算連續 3 個字母段數的數量就是答案。

```cpp
class Solution {
public:
    /**
     * @param S: a string
     * @return:  return the minimum number of moves
     */
    int MinimumMoves(string &S) {
        // write your code here
        char temp;
        int count = 0;
        int res = 0;

        for (int i=0; i < S.size(); i++){
            if(temp == NULL){
                temp = S[i];
            }

            if(temp == S[i]){
                count++;
            }else{
                temp = S[i];
                count = 1;
            }

            if(count == 3){
                count = 0;
                res++;
            }
        }

        return res;
    }
};
```

