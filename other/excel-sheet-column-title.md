---
description: 注意、Mircosoft
---

# Excel Sheet Column Title

[1350. Excel Sheet Column Title](https://www.lintcode.com/problem/excel-sheet-column-title/?_from=ladder&&fromId=21)

#### 題目

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

Example Example1

```text
Input: 28
Output: "AB"
```

Example2

```text
Input: 29
Output: "AC"
```

Notice

```text
1 -> A
2 -> B
3 -> C
 ...
26 -> Z
27 -> AA
28 -> AB
```

#### 解法

> 注意下述解的 n-1，不能用 n 來處理。會死在 26\*26 這種案例上

```cpp
class Solution {
public:
    /**
     * @param n: a integer
     * @return: return a string
     */
    string convertToTitle(int n) {
        // write your code here
        if(n <= 0){
            return "";
        }

        string res = "";
        // 不太需要，因為可以用  (char)(1 + 'A') => B
        // std::vector<char> letters = { ' ', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z' };

        while(n > 0){
            char unit = (char) ( (n-1) % 26 + 'A' );
            res = unit + res;
            n = (n-1) / 26;
        }

        return res;
    }
};
```

