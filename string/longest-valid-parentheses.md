---
description: 注意、Amazon
---

# Longest Valid Parentheses

[193. Longest Valid Parentheses](https://www.lintcode.com/problem/longest-valid-parentheses/?_from=ladder&&fromId=131)

#### 題目

Given a string containing just the characters '\(' and '\)', find the length of the longest valid \(well-formed\) parentheses substring.

Example Example 1:

```text
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

Example 2:

```text
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

#### 解法 - DP

**Step 1. 確認狀態**

```text
dp[i] = 從 s[0] 至 s[i] 的 Max Longest Vaild Parenthese
```

**Step 2. 轉移方程式**

假設 s\[i\] = '\)' 的情況，它可能會產生兩種情況。

情況 1 : s\[i-1\] = '\(' 那這樣轉移方程式為

```text
if (s[i] == ')' && s[i-1] == '(') 

dp[i] = dp[i-2] + 2
ex. xxxxxx()
```

情況 2 :

```text
ex. ()()(xxxxx) 
s[i] = ')'
s[j] = '('
```

這裡的問題在於要如何找到 j 的位置。

```cpp
class Solution {
public:
    /**
     * @param s: a string
     * @return: return a integer
     */
    int longestValidParentheses(string &s) {
        // write your code here
        int n = s.size();
        std::vector<int> dp(n, 0);
        int maxLVP = 0;
        for (int i=1; i < n; i++){
            if(s[i] == ')'){
                int j = i - dp[i-1];
                if(s[i-1] == '('){
                    // 情況 1 : xxxx()
                    dp[i] = ((i >= 2) ? dp[i-2] : 0) + 2;
                }else if(j > 0 && s[j-1] == '('){
                    // 情況 2 : ()(xxxx)
                    dp[i] = dp[i-1] + (( j >= 2 ) ? dp[j-2] : 0 ) + 2; 
                }
            }
            maxLVP = max(maxLVP, dp[i]);
        }
        return maxLVP;
    }
};
```

#### 解法 - Stack

```text
")(()(()(((())(((((()()))((((()()(()()())())())()))()()()())(())()()(((()))))()((()))(((())()((()()())((())))(())))())((()())()()((()((())))))((()(((((()((()))(()()(())))((()))()))())"
```

```cpp
class Solution {
public:
    /**
     * @param s: a string
     * @return: return a integer
     */
    int longestValidParentheses(string &s) {
        // write your code here
        std::stack<int> stack;
        int res = 0;
        int start = 0;

        for (int i=0; i < s.size(); i++){
            if(s[i] == '('){
                stack.push(i);
            }else{
                if(stack.empty()){
                    start = i + 1;
                }else{
                    stack.pop();
                    res = stack.empty() ? max(res, i - start + 1) : max(res , i - stack.top());
                }
            }
        }
        return res;
    }
};
```

