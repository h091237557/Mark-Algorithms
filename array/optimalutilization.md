---
description: 注意、Amazon
---

# optimalUtilization

[1797. optimalUtilization](https://www.lintcode.com/problem/optimalutilization/?_from=ladder&&fromId=87)

#### 題目

Give two sorted arrays. To take a number from each of the two arrays, the sum of the two numbers needs to be less than or equal to k, and you need to find the index combination with the largest sum of the two numbers. Returns a pair of indexes containing two arrays. If you have multiple index answers with equal sum of two numbers, you should choose the index pair with the smallest index of the first array.

1. The sum of the two numbers &lt;= k
2. The sum is the biggest
3. Both array indexes are kept to a minimum

Example Example1

```text
A = [1, 4, 6, 9], B = [1, 2, 3, 4], K = 9
return [2, 2]
```

Example2:

```text
Input: 
A = [1, 4, 6, 8], B = [1, 2, 3, 5], K = 12
Output:
[2, 3]
```

#### 解法 - 錯誤

這一題是問說，給定兩組已排序順組，從這兩個數組中尋出最接近 K 的兩個數合的位置。

這一題我們可以使用兩個指針分從頭開始跑起，規則如下 :

1. if \(A\[i\] + B\[j\]\) &lt; 9 =&gt; i++ j++
2. if \(A\[i\] + B\[j\]\) &gt; 9 =&gt; break 
3. if\(A\[i\] &gt; B\[j\]\) j-- than i++ =&gt; 這兩個應該就是答案

這個會在以下案例錯誤，雖然 \[2,2\] 也對，但 index 要選小的 :

```text
Input
[1,4,6,8]
[1,2,3,5]
9
Output
[2,2]
Expected
[1,3]
```

```cpp
class Solution {
public:
    /**
     * @param A: a integer sorted array
     * @param B: a integer sorted array
     * @param K: a integer
     * @return: return a pair of index
     */
    vector<int> optimalUtilization(vector<int> &A, vector<int> &B, int K) {
        // write your code here
        int i = 0;
        int j = 0;
        vector<int> res;
        while(i < (A.size()-1) || j < (B.size()-1)){
            if((A[i] + B[j]) == K){
                res.push_back(i);
                res.push_back(j);
                return res;
            }else if((A[i] + B[j]) < K){
                i++;
                j++;
            }else{
                break;
            }
        }

        if(A[i] + B[j] > K){
            if(A[i] > B[j]){
                i--;
            }else{
                j--;
            }
        }

        res.push_back(i);
        res.push_back(j);
        return res;
    }
};
```

#### 解法

**Corner Case**

```text
Input
[1, 1, 3, 3, 6, 11, 12, 14, 15, 18]
[5, 5, 9, 12, 18]
9
Output
[3,1]
Expected
[2,0]
```

```cpp
class Solution {
public:
    /**
     * @param A: a integer sorted array
     * @param B: a integer sorted array
     * @param K: a integer
     * @return: return a pair of index
     */
    vector<int> optimalUtilization(vector<int> &A, vector<int> &B, int K) {
        // write your code here
        vector<int> res;

        if(A.size() == 0 || B.size() == 0){
            return res;
        }


        int i = 0;
        int j = B.size() - 1;
        int maxSum = INT_MIN;
        pair<int,int> minIndex;

        while(i < A.size() - 1 && j >= 0){

            int sum = A[i] + B[j];
            if(sum == K){
                // ! 要有這一段，不然上面 case 會死
                while(j > 0 && B[j] == B[j-1]){
                    j--;
                }
                res.push_back(i);
                res.push_back(j);
                return res;
            }

            if(sum < K){
                if(sum > maxSum){
                    maxSum = sum;

                    // ! 同上
                    while(j > 0 && B[j] == B[j-1] ){
                        j--;
                    }
                    minIndex = {i,j};
                }
                i++;
            }else{
                j--;
            }
        }

        res.push_back(minIndex.first);
        res.push_back(minIndex.second);

        return res;
    }
};
```

