---
description: +
---

# Prime Number of Set Bits in Binary Representation

[1046. Prime Number of Set Bits in Binary Representation](https://www.lintcode.com/problem/prime-number-of-set-bits-in-binary-representation/?_from=ladder&&fromId=15)

#### 題目

Given two integers L and R, find the count of numbers in the range \[L, R\] \(inclusive\) having a prime number of set bits in their binary representation.

\(Recall that the number of set bits an integer has is the number of 1s present when written in binary. For example, 21 written in binary is 10101 which has 3 set bits. Also, 1 is not a prime.\)

Example Example 1:

```text
Input: L = 6, R = 10
Output: 4
Explanation:
6 -> 110 (2 set bits, 2 is prime)
7 -> 111 (3 set bits, 3 is prime)
9 -> 1001 (2 set bits , 2 is prime)
10->1010 (2 set bits , 2 is prime)
```

Example 2:

```text
Input: L = 10, R = 15
Output: 5
Explanation:
10 -> 1010 (2 set bits, 2 is prime)
11 -> 1011 (3 set bits, 3 is prime)
12 -> 1100 (2 set bits, 2 is prime)
13 -> 1101 (3 set bits, 3 is prime)
14 -> 1110 (3 set bits, 3 is prime)
15 -> 1111 (4 set bits, 4 is not prime)
```

#### 解法

這一題是要求，給定上限 R 與下限 L 問在 R -&gt; L 得數字轉成 2 進位，然後判斷 1 的數量是否為值數，請輸出總共有幾個值數。

這題我們可以使用以下的方法來計算某數 2 進數 1 的數字。

首先我們先看以下的公式 x & x - 1 這個方法可以幫我們消除最右邊的 1，這也代表我們只要計算要將 1 全部消失所需次數，那它就是 1 的總數。

```text
x & (x - 1) 用于消去x最后一位的1, 比如x = 12, 那么在二进制下就是(1100)2

x           = 1100
x - 1       = 1011
x & (x - 1) = 1000
```

[九章微課堂](https://www.jiuzhang.com/tutorial/bit-manipulation/82)

**那這裡要如何判斷值數呢 ?**

這裡題目有給定 l 與 r 的範圍是 :

> \[1, 10^6\]

那是事實上咱們就可得知，它最多只有 19 個位數，所以 19 以內的值數有 :

> 2,3,5,7,11,13,17,19

```cpp
class Solution {
private:
    int countBit(int num){
        int res = 0;

        while(num != 0){
            num = (num & num - 1);
            res++;
        }

        return res;
    }
public:
    /**
     * @param L: an integer
     * @param R: an integer
     * @return: the count of numbers in the range [L, R] having a prime number of set bits in their binary representation
     */
    int countPrimeSetBits(int L, int R) {
        // Write your code here
        int l = L;
        int r = R;
        vector<int> primes{2,3,5,7,11,13,17,19};
        int len = r + 1;
        int res = 0; 

        for (int i=l; i<= r; i++){
            int count = countBit(i);
            for (int j=0; j < primes.size(); j++){
                if(count == primes[j]){
                    res++;
                }
            }
        }

        return res; 
    }
};
```

