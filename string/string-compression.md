---
description: 注意、Amazon
---

# String Compression

[213. String Compression](https://www.lintcode.com/problem/string-compression/?_from=ladder&&fromId=59)

#### 題目

Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3.

If the "compressed" string would not become smaller than the original string, your method should return the original string.

You can assume the string has only upper and lower case letters \(a-z\).

Example Example 1:

```text
Input: str = "aabcccccaaa"
Output: "a2b1c5a3"
```

Example 2:

```text
Input: str = "aabbcc"
Output: "aabbcc"
```

#### 解法

```cpp
class Solution {
public:
    /**
     * @param originalString: a string
     * @return: a compressed string
     */
    string compress(string &originalString) {
        // write your code here

        string s = originalString;
        char temp = s[0];
        int count = 0;
        string res = "";
        for (int i=0; i < s.size(); i++){
            if(s[i] == temp){
                count++;
            }else{
                // 先組合文字
                res += (temp + to_string(count));
                temp = s[i];
                count = 1;
            }
        }

        if(count > 0){
            res += (temp + to_string(count));
        }
        return (res.size() < s.size()) ? res : originalString;
    }
};
```

