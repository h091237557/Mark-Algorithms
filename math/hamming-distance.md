---
description: July30
---

# Hamming Distance

[461. Hamming Distance](https://leetcode.com/problems/hamming-distance/)

#### 題目

The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers x and y, calculate the Hamming distance.

Note: 0 ≤ x, y &lt; 2^31.

```text
Example:

Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

**題目解析**

求兩個數 x 與 y 的 Hamming Distance，它的定義如下 :

> 兩個數轉成二進位後，有幾個是不同的

```text
2 => 0010
5 => 0101

Hamming Distance = 3(右邊 3 位都是不同的)
```

#### 解法 - Bit Operation

```text
Time Complexity: O(m (xor 後 1 的數量))
Space Complexity: O(1)
```

這題簡單想到的方法為，將兩數進行 xor，然後在計算有幾個 1。

```text
0010
0101
xor
0111

總共有 3 個 1。
```

計算 1 的方法為 :

```text
x & (x-1) => 可消失最後一個 1。
然後再計算執行了幾次才變為 0。
```

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int temp = x^y;
        int res = 0;
        while(temp != 0){
            res++;
            temp &= (temp-1);
        }
        return res;
    }
};
```

