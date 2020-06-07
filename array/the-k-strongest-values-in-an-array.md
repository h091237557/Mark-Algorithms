---
description: Contest 192
---

# The k Strongest Values in an Array

[5429. The k Strongest Values in an Array](https://leetcode.com/problems/the-k-strongest-values-in-an-array/)

#### 題目

Given an array of integers arr and an integer k.

A value arr\[i\] is said to be stronger than a value arr\[j\] if \|arr\[i\] - m\| &gt; \|arr\[j\] - m\| where m is the median of the array. If \|arr\[i\] - m\| == \|arr\[j\] - m\|, then arr\[i\] is said to be stronger than arr\[j\] if arr\[i\] &gt; arr\[j\].

Return a list of the strongest k values in the array. return the answer in any arbitrary order.

Median is the middle value in an ordered integer list. More formally, if the length of the list is n, the median is the element in position \(\(n - 1\) / 2\) in the sorted list \(0-indexed\).

* For arr = \[6, -3, 7, 2, 11\], n = 5 and the median is obtained by sorting the array arr = \[-3, 2, 6, 7, 11\] and the median is arr\[m\] where m = \(\(5 - 1\) / 2\) = 2. The median is 6.
* For arr = \[-7, 22, 17, 3\], n = 4 and the median is obtained by sorting the array arr = \[-7, 3, 17, 22\] and the median is arr\[m\] where m = \(\(4 - 1\) / 2\) = 1. The median is 3.

```text
Input: arr = [1,2,3,4,5], k = 2
Output: [5,1]
Explanation: Median is 3, the elements of the array sorted by the strongest are [5,1,4,2,3]. The strongest 2 elements are [5, 1]. [1, 5] is also accepted answer.
Please note that although |5 - 3| == |1 - 3| but 5 is stronger than 1 because 5 > 1.

Input: arr = [1,1,3,5,5], k = 2
Output: [5,5]
Explanation: Median is 3, the elements of the array sorted by the strongest are [5,5,1,1,3]. The strongest 2 elements are [5, 5].

Input: arr = [6,7,11,7,6,8], k = 5
Output: [11,8,6,6,7]
Explanation: Median is 7, the elements of the array sorted by the strongest are [11,8,6,6,7,7].
Any permutation of [11,8,6,6,7] is accepted.

Input: arr = [6,-3,7,2,11], k = 3
Output: [-3,11,2]

Input: arr = [-7,22,17,3], k = 2
Output: [22,17]
```

Constraints:

* 1 &lt;= arr.length &lt;= 10^5 
* -10^5 &lt;= arr\[i\] &lt;= 10^5 
* 1 &lt;= k &lt;= arr.length

**題目解析**

這題是問說，給定一個數組，求出前 k 個最強的值。而 strongest 的定義如下 :

```text
if |arr[i] - m| >= |arr[j] - m| 則可以說 arr[i] strong than arr[j]
其中 m 為中位數

Ex. arr = [1,2,3,4,5], k = 2, m = (5-1)/2 = 2 index = 3

|5 - 3| 
|1 - 3|
是最強的兩個，所以輸出 [5,1]
```

#### 解法 - 雙指針 O\(NlogN\)

這題比較簡單的想法就是，先將數組進行排序後，在從 left 與 right 找尋 strongest 的值。

```cpp
class Solution {
public:
    vector<int> getStrongest(vector<int>& nums, int k) {
        vector<int> res;
        sort(nums.begin(), nums.end());
        int median = nums[(nums.size()-1)/2];
        int left = 0;
        int right = nums.size() - 1;

        while(k > 0){
            if(abs(nums[right] - median) >= abs(nums[left] - median)){
                res.push_back(nums[right]);
                right--;
            }else{
                res.push_back(nums[left]);
                left++;
            }
            k--;
        }
        return res;
    }
};
```

