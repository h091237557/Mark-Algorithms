---
description: 注意、Amazon、Mircosoft
---

# Count Primes

[1324. Count Primes](https://www.lintcode.com/problem/count-primes/?_from=ladder&&fromId=15)

#### 題目

Count the number of prime numbers less than a non-negative number, n.

Example Example 1：

```text
Input: n = 2
Output: 0
```

Example 2：

```text
Input: n = 4
Output: 2
Explanation：2, 3 are prime number
```

#### 解法 - 爆力解 \( 但會 timeout \)

這一題是問說，給你一個 n，請計算 1 ~ n 有幾個質數。而質數的定義如下 :

> 一個數，除了 1 與自身外，它不能被任何的數整除

所以這一題最簡單的想法就是，每一個數為 i 要判斷它是否為質數，從從 1 至 i 都除一次，如果有一個 i % n 為 0 且不等於 1 或自身，則代表它不是質數。

但這裡要注意幾個重點 :

* 1 不是質數
* 2、3 是質數

```cpp
class Solution {
public:
    /**
     * @param n: a integer
     * @return: return a integer
     */
    int countPrimes(int n) {
        // write your code here

        int res = 0;

        bool isPrimeFlag = true;
        for (int i=2; i < n; i++){
            isPrimeFlag = true;
            if(i == 2 || i == 3){
                res++;
                continue;
            }

            for (int j=2; j <= i; j++){
                if( i != j && i % j == 0){
                    isPrimeFlag = false;
                    break;
                }
            }

            if(isPrimeFlag){
                res++;
            }
        }
        return res;
    }
};
```

#### 解法 - 埃拉托斯特尼篩法

[參考資料](https://zh.wikipedia.org/wiki/%E5%9F%83%E6%8B%89%E6%89%98%E6%96%AF%E7%89%B9%E5%B0%BC%E7%AD%9B%E6%B3%95)

> 先用2去篩，即把2留下，把2的倍數剔除掉；再用下一個質數，也就是3篩，把3留下，把3的倍數剔除掉；接下去用下一個質數5篩，把5留下，把5的倍數剔除掉；不斷重複下去......。

```cpp
class Solution {
public:
    int countPrimes(int n) {
        // edge case 
        if(n <= 1){
            return 0;
        }

        /* Sieve of Eratosthenes */
        vector<bool> isNotPrime(n, false);
        int res = 0;

        // 這裡不用 i <= n 的原因為 
        // 如果 n = 7 則要求的答案為 3 ( 2,3,5 ) ，它自已本身不算。
        for (int i=2 ; i < n; i++){
            if(isNotPrime[i]) continue;
            res++;
            for (int j=2 ; i*j < n; j++){
                isNotPrime[i*j] = true;
            }
        }
        return res;
    }
};
```

