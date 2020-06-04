---
description: 注意、Microsoft
---

# Excel Sheet Column Number

[1348. Excel Sheet Column Number](https://www.lintcode.com/problem/excel-sheet-column-number/?_from=ladder&&fromId=115)

#### 題目

Given a column title as appear in an Excel sheet, return its corresponding column number.

Example Example1

```text
Input: "AB"
Output: 28
```

Example2

```text
Input: "AC"
Output: 29
```

Notice A -&gt; 1 B -&gt; 2 C -&gt; 3 ... Z -&gt; 26 AA -&gt; 27 AB -&gt; 28

#### 解法

* 從最左邊個數開始計算 sum。
* 每進一位就公式就變成

```text
n => 位置

26^(n) * (1~26)  + 26^(n-1) * (1~26)  + 26^(0) * (1~26) ....
```

```cpp
class Solution {
public:
    /**
     * @param s: a string
     * @return: return a integer
     */
    int titleToNumber(string &s) {
        // write your code here
        int res = 0;
        int n = s.size();
        int unit = n;

        while(unit != 0){
            res += (pow(26, n - unit) * ((int) s[unit-1] - 64)); 
            unit--;
        }

        return res;
    }
};
```

