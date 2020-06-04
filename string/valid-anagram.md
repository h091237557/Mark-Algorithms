---
description: 注意、Amazon
---

# Valid Anagram

[158. Valid Anagram](https://www.lintcode.com/problem/valid-anagram/?_from=ladder&&fromId=59)

#### 題目

Write a method anagram\(s,t\) to decide if two strings are anagrams or not.

Example Example 1:

```text
Input: s = "ab", t = "ab"
Output: true
```

Example 2:

```text
Input:  s = "abcd", t = "dcba"
Output: true
```

Example 3:

```text
Input:  s = "ac", t = "ab"
Output: false
```

Challenge O\(n\) time, O\(1\) extra space

Clarification What is Anagram?

Two strings are anagram if they can be the same after change the order of characters.

#### 解法 - ASCII 錯誤

這個會死在，以下的測試。

```text
"az"
"by"
```

```cpp
class Solution {
public:
    /**
     * @param s: The first string
     * @param t: The second string
     * @return: true or false
     */
    bool anagram(string &s, string &t) {
        // write your code here
        int s_count = 0;
        int t_count = 0;
        for (int i=0; i < s.size(); i++){
            s_count += (int) s[i];
            t_count += (int) t[i];
        }
        return s_count == t_count;
    }
};
```

#### 解法 - HaspMap

```cpp
class Solution {
public:
    /**
     * @param s: The first string
     * @param t: The second string
     * @return: true or false
     */
    bool anagram(string &s, string &t) {
        // write your code here
        int set_s[256]={0}, set_t[256]={0};
        for(int i = 0; i < s.length(); i++)
            set_s[s[i]] ++;
        for(int i = 0; i < t.length(); i++)
            set_t[t[i]] ++;
        for(int i = 0; i < 256; i++)
            if(set_s[i] != set_t[i]) 
                return false;
        return true;
    }
};
```

#### Challenge 正解 \( 牛 \)

這個解法有個重要的知識

**對一個數 ^ 兩次等於沒有操作**

```text
a ^ b ^ b = a
```

```text
Ex.

a = 5 (101)
b = 2 (010)

a ^ b ^ b = 101 ^ 010 ^ 010 = (111) ^ (010) = 101 = 5
```

```cpp
class Solution {
public:
    /**
     * @param s: The first string
     * @param t: The second string
     * @return: true or false
     */
    bool anagram(string &s, string &t) {
        // write your code here
        int hash = 0;
        int temp = 0;

        // tip: 這裡不能用單個 s[i] % 26 來當 hash 值
        // 因為這裡在 az 與 by 這種案例會錯。因為加總起來會消。
        for (int i=0; i < s.size(); i++){
            temp ^= s[i];
            hash += s[i]*s[i] % 26;
        }

        for (int j=0; j < t.size(); j++){
            temp ^= t[j];
            hash -= t[j]*t[j] % 26;
        }

        return temp == 0 && hash == 0;
    }
};
```

