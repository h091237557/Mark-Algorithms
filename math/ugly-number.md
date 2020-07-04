# Ugly Number

[263. Ugly Number](https://leetcode.com/problems/ugly-number/)

```text
小知識

任何數都是由質數組成的

2 = 2^2
24 = 2^3 x 3^1
720 = 2^4 x 3^2 x 5

ugly number = 2^i x 3^j x 5^k
```

#### 題目

Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5.

Example 1:

```text
Input: 6
Output: true
Explanation: 6 = 2 × 3
Example 2:
```

```text
Input: 8
Output: true
Explanation: 8 = 2 × 2 × 2
Example 3:
```

```text
Input: 14
Output: false 
Explanation: 14 is not ugly since it includes another prime factor 7.
```

#### 思路

time: O\(log2n + log3n + log5n\) space: O\(1\)

```text
Runtime: 4 ms, faster than 93.73% of C++ online submissions for Ugly Number.
Memory Usage: 8.3 MB, less than 5.12% of C++ online submissions for Ugly Number.
```

```cpp
class Solution {
public:
    bool isUgly(int num) {
        if(num == 0) return false;

        while(num % 2 == 0) { num /= 2;};
        while(num % 3 == 0) { num /= 3;};
        while(num % 5 == 0) { num /= 5;};

        return num == 1;
    }
};
```

