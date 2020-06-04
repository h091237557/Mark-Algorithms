---
description: 注意、Microsoft
---

# Min Deletions To Obtain String in Right Format

[1821. Min Deletions To Obtain String in Right Format](https://www.lintcode.com/problem/min-deletions-to-obtain-string-in-right-format/?_from=ladder&&fromId=143)

#### 題目

We are given a string SS of length NN consisting only of letters A and/or B. Our goal is to obtain a string in the format A...AB...B. \(all letters A occur before all letters B\) by deleting some letters from SS. In particular, strings consisting only of letters A or only of letters B fit this format.

Write a function that, given a string SS, return the minimum number of letters that need to be deleted from SS in order to obtain a string in the above format.

Example Example 1

```text
Input: "BAAABAB"
Output: 2
Explanation: We can obtain "AAABB" by deleting the first occurrence of 'B' and the last occurrence of 'A'.
```

Example 2

```text
Input: "BBABAA"
Output: 3
Explanation: We can delete all occurrences of 'A' or 'B'.
```

Example 3

```text
Input: "AABBBB"
Output: 0
Explanation: We do not have to delete any letters, because the given string is already in the expected format.
```

Notice

* NN is an integer within the range \[1,100\,000\]\[1,100000\];
* string SS consists only of the characters A and/or B.

#### 解法

這裡是說給定一組只有 A 與 B 的字串，然後藉由移除某個字元的方式，來產生 AAA..AB...B 這種格式，也就是前 A 後 B 的模式。問最多需要移除幾次。

這個解法為計算每一個位置的右邊 A 的數量與左邊 B 的數量。然後找出最小的。如果範例，計算出每個位置所需要次，然後在找最小值。

```text
"BAAABAB"

位置 1
A = 4
B = 1
預期變成 => "ABBBBBB"
5 次

位置 2
A = 3
B = 1
預期變成 => "AABBBBB"
4 次

...
```

```cpp
class Solution {
public:
    /**
     * @param s: the string
     * @return: Min Deletions To Obtain String in Right Format
     */
    int minDeletionsToObtainStringInRightFormat(string &s) {
        // write your code here

        // 尋找某個位置的右邊 A 的數量與左邊 B 的數量最小。
        int aCount = 0;
        int bCount = 0;
        int res = 0;
        for (int i=0; i < s.size(); i++){
            if(s[i] == 'A'){
                aCount++;
            }
        }

        res = aCount;

        for (int j=0; j < s.size(); j++){
            if(s[j] == 'A'){
                aCount--;
            }else{
                bCount++;
            }

            res = min(res, aCount + bCount);
        }

        return res;
    }
};
```

