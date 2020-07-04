---
description: July30
---

# Super Ugly Number

[313. Super Ugly Number](https://leetcode.com/problems/super-ugly-number/)

#### 題目

Write a program to find the nth super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the given prime list primes of size k.

Example:

```text
Input: n = 12, primes = [2,7,13,19]
Output: 32 
Explanation: [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 
             super ugly numbers given primes = [2,7,13,19] of size 4.
```

* 1 is a super ugly number for any given primes.
* The given numbers in primes are in ascending order.
* 0 &lt; k ≤ 100, 0 &lt; n ≤ 106, 0 &lt; primes\[i\] &lt; 1000.
* The nth super ugly number is guaranteed to fit in a 32-bit signed integer.

**題目解析**

這題一樣是求 ugly number，前面幾題的 ugly number 公式如下 :

> ugly number = 2^n x 3^n x 5^n

而 super ugly number 會多給你一個 primes，因此公式如下 :

> super ugly number = primes\[0\]^n x primes\[1\]^n x ... x primes\[m-1\]^n

#### 解法 - 爆力法

```text
N : n-th
M : primes 的長度
Time Complexity: O(N*M)
Space Complexity: O(1)
```

```cpp
class Solution {
private:
    bool isSuperUgly(int number, vector<int>& primes){
        if(number == 1){
            return true;
        }

        for (int i=0; i < primes.size(); i++){
            while(number % primes[i] == 0) number /= primes[i];
        }
        return number == 1;
    }
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        int number = 0;
        while(n != 0){
            number++;
            if(isSuperUgly(number, primes)){
                n--;
            }
        }
        return number;
    }
};
```

#### 解法 - 公式解

方法如同 ugly number II。

```cpp
class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        vector<int> res;
        res.push_back(1);
        // def: 儲放 ids[i] 代表 primes[i] 的次數數
        vector<int> idx(primes.size(), 0);  

        while(res.size() < n){
            int ugly = INT_MAX;
            for (int i=0; i < primes.size(); i++){
                ugly = min(ugly, res[idx[i]]*primes[i]);
            }
            res.push_back(ugly);

            for (int j=0; j < primes.size(); j++){
                if(ugly == primes[j]*res[idx[j]]){
                    idx[j]++;
                }
            }
        }
        return res[n-1];
    }
};
```

