---
description: Amazon
---

# Happy Number

[488. Happy Number](https://www.lintcode.com/problem/happy-number/description?_from=ladder&&fromId=131)

#### 題目

Write an algorithm to determine if a number is happy.

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 \(where it will stay\), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example Example 1:

```text
Input:19
Output:true
Explanation:

19 is a happy number

    1^2 + 9^2 = 82
    8^2 + 2^2 = 68
    6^2 + 8^2 = 100
    1^2 + 0^2 + 0^2 = 1
```

Example 2:

```text
Input:5
Output:false
Explanation:

5 is not a happy number

25->29->85->89->145->42->20->4->16->37->58->89
89 appears again.
```

以上圖範例來看 28 與 32 為 happy number 而 37 則否。

#### 解法

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: true if this is a happy number or false
     */
    bool isHappy(int n) {

        // edge case
        if(n == 0){
            return false;
        }
        if(n == 1){
            return true;
        }

        unordered_map<int, bool> map;
        map[n] = true;

        while(true){
            int s = 0;
            while(n){
                s += (n % 10) * (n % 10);
                n = n/10;
            }

            if(s == 1){
                return true;
            }else{
                if(map.count(s)){
                    return false;
                }
                map[s] = true; 
                n = s;
            }
        }

        return false;
    }
};
```

