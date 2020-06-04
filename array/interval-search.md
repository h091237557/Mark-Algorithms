---
description: Amazon
---

# Interval Search

[1564. Interval Search](https://www.lintcode.com/problem/interval-search/?_from=ladder&&fromId=62)

#### 題目

Given a List of intervals, the length of each interval is 1000, such as \[500,1500\], \[2100,3100\].Give a number arbitrarily and determine if the number belongs to any of the intervals.return True or False.

Example Example1

```text
Input: List = [[100,1100],[1000,2000],[5500,6500]] and number = 6000
Output: true
Explanation: 
6000 is in [5500,6500]
```

Example2

```text
Input: List = [[100,1100],[2000,3000]] and number = 3500
Output: false
Explanation: 
3500 is not in any list's interval
```

#### 解法

```cpp
class Solution {
public:
    /**
     * @param intervalList:
     * @param number:
     * @return: return True or False
     */
    string isInterval (vector<vector<int>> &intervalList, int number) {
        // Write your code here
        for (int j = 0; j < intervalList.size(); j++) {
            vector<int> i = intervalList[j];
            if (number >= i[0] && number <= i[1]) {
                return "True";
            }
        }
        return "False" ;
    }
};
```

