---
description: Amazon
---

# Valid Parentheses

[423. Valid Parentheses](https://www.lintcode.com/problem/valid-parentheses/description?_from=ladder&&fromId=15)

#### 題目

Given a string containing just the characters '\(', '\)', '{', '}', '\[' and '\]', determine if the input string is valid.

The brackets must close in the correct order, "\(\)" and "\(\)\[\]{}" are all valid but "\(\]" and "\(\[\)\]" are not.

Example Example 1:

```text
Input: "([)]"
Output: False
```

Example 2:

```text
Input: "()[]{}"
Output: True
```

#### 解法 - Stack

這題是問說，給定一個括號組成的字串，判斷是不是 Parentheses。

```text
"([])" => true
"([)]" => false
"()[]{}" => true
```

這題可以使用 stack 這個資料結構來幫忙我們處理，流程如下 :

1. 如果是 \(、\[、{ 的字串，就將他丟到 stack 中。
2. 而如果是 \)、\]、} 的字串，則從 stack 中取出一個括號。
3. 比對看看是否為相對應的括號。如果否這不為 Parentheses。
4. 是的話則繼續 step 1 ~ 3。

```cpp
class Solution {
public:
    /**
     * @param s: A string
     * @return: whether the string is a valid parentheses
     */
    bool isValidParentheses(string &s) {
        // write your code here

        std::stack<char> stack;
        int res = true;

        for (int i=0; i < s.size(); i++){
            if(s[i] == '(' || s[i] == '[' || s[i] == '{'){
                stack.push(s[i]);
            }else{
                if(stack.empty()){
                    res = false;
                    break;
                }

                char bracket = stack.top();
                stack.pop();
                if(s[i] == ')' && bracket == '('){
                    continue;
                }
                if(s[i] == ']' && bracket == '['){
                    continue;
                }
                if(s[i] == '}' && bracket == '{'){
                    continue;
                }
                res = false;
                break;
            } 
        }

        if(!stack.empty()){
            res = false;
        }

        return res;
    }
};
```

