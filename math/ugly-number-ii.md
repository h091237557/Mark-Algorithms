---
description: July30
---

# Ugly Number II

[Ugly Number II](https://leetcode.com/explore/featured/card/july-leetcoding-challenge/544/week-1-july-1st-july-7th/3380/)

#### 題目

Write a program to find the n-th ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5.

Example:

```text
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```

Note:

* 1 is typically treated as an ugly number.
* n does not exceed 1690.

**題目解析**

題目是說，給定一個 n，請找出 n-th 個 ugly number。其中 ugly number 的定義如下 :

> ugly number 之能質因素 2、3、5 組成
>
> ugly number = 2^n x 3^n x 5^n

#### 解法思路 - 爆力法

```text
Time Complexity: O(N)
Space Complexity: O(1)
```

這題最簡單的解法為，從 1 到 ? 來找第 n-th 個 ugly number，程式碼流程如下:

```text
function getNthUglyNo(n)
   int number = 1;
   while(n != 0){
      if(isUgly(number)){
         n--;
      }
      number++;
   }
   return number
```

```cpp
class Solution {
private:
    bool isUgly(int number){
        if(number == 1){
            return true;
        }

        while(number % 2 == 0) number /= 2;
        while(number % 3 == 0) number /= 3;
        while(number % 5 == 0) number /= 5;
        return number == 1;
    }    
public:
    int nthUglyNumber(int n) {
        int number = 0;
        while(n != 0){
            number++;
            if(isUgly(number)){
                n--;
            }
        }
        return number;
    }
};
```

#### 解法 - Prioriy Queue

```text
Time Complexity: O(NlogN)
Space Complexity: O(N)
```

```cpp
class Solution {
private:
    struct Compare{
        bool operator ()(long long a, long long b){
            return a > b;
        }
    };    
public:
    int nthUglyNumber(int n) {
        int res = 0;
        priority_queue<long long, vector<long long>, Compare> pq;
        pq.push(1);
        while(n != 0){
            long long cur = pq.top();
            pq.pop();
            // tip: 記得處理重複的 例如 2*3, 3*2 都為 6 的情況
            if(cur == res){
                continue;
            }
            n--;

            res = cur;
            pq.push(2*cur);
            pq.push(3*cur);
            pq.push(5*cur);
        }
        return res;
    }
};
```

#### 解法 - 公式解

```text
Time Complexity: O(N)
Space Complexity: O(N)
```

ugly number 的公式如下 :

> ugly number = 2^n x 3^n x 5^n

然後可以使用以下的方式來進行計算。

```text
min
{2,3,5} => 2
{2*2,3,5} => 3
{2*2,3*2,5} => 4
{2*3,3*2,5} => 5
{2*3,3*2,5*2} => 6
{2*4,3*3,5*2} => 8
{2*5,3*3,5*2} => 9
{2*5,3*4,5*2} => 10
{2*6,3*4,5*3} => 12
```

```text
r1 => min 2*1 => res[1] = 2 => i=1
r2 => min 3*1 => res[2] = 3 => j=1
r3 => min({2*2},{3*2},{5*1}) => res[3] = 4 => i=2
r4 => min({2*3,{3*2},{5*1}}) => res[4] = 5 => k=1
r5 => min({2*3},{3*2},{5*2}) => res[5] = 6 => i=3,j=2
```

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {

        vector<long long> res;
        res.push_back(1);
        int i=0;
        int j=0;
        int k=0;

        while(res.size() < n){
            long long ugly = min(2*res[i], min(3*res[j], 5*res[k]));
            res.push_back(ugly);

            if(ugly == 2*res[i]) i++;
            if(ugly == 3*res[j]) j++;
            if(ugly == 5*res[k]) k++;
        }
        return res[n-1];
    }
};
```

