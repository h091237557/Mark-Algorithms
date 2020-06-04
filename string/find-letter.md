---
description: Microsoft
---

# Find Letter

[1820. Find Letter](https://www.lintcode.com/problem/find-letter/?_from=ladder&&fromId=143)

#### 題目

Given a string str, return the letter with the largest alphabetical order in the string and both such uppercase and lowercase letters must appear in the string. If no such letter exists, return '~'.

Example Example 1:

```text
Input:"aAbBcD"
Output:'B'
Explanation：Because C and D's lowercase and uppercase do not appear at the same time, 
both A and B are appearing, but B is larger than A, so B is returned.
```

Example 2:

```text
Input:"looGVSSPbR"
Output:'~'
```

please return uppercase \|str\|&lt;=1000

#### 解法

這裡是問說給定一個字串 S 找出以下條件的字母

* 大寫與小寫同時出現
* 如果有兩個字母相同，則根據 alphabetical order 大的來選，ex A 與 B 擇選 B。
* 如果都找不到，則回傳 ~ 。

這裡的解法為 :

1. 建立一個二維 vector 用來儲放每個大小寫字母的出現次數
2. 每當某個字母大小寫都出現時，如果 alphabetical order 較大，則更新 ans。
3. ans 代表某個字母的 index，最後留小來的就是最大 alphabetical order 的字母。

```cpp
class Solution {
public:
    /**
     * @param str: the str
     * @return: the letter
     */
    char findLetter(string &str) {
        // Write your code here.
        int ans = -1;
        vector<vector<int>> tables(2, vector<int>(26));

        for (int i=0; i < str.size(); i++){
            int index;
            if(str[i] >= 'a' && str[i] <= 'z'){
                index = str[i] - 'a';
                tables[0][index] = 1;
            }else{
                index = str[i] - 'A';
                tables[1][index] = 1;
            }

            if(tables[0][index] == 1 && tables[1][index] == 1 && index > ans){
                ans = index;
            }
        }

        if(ans == -1){
            return '~';
        }

        return 'A' + ans;
    }
};
```

