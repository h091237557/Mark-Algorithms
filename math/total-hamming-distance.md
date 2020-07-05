# Total Hamming Distance



[477. Total Hamming Distance](https://leetcode.com/problems/total-hamming-distance/)

#### 題目

The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Now your job is to find the total Hamming distance between all pairs of the given numbers.

Example:

```text
Input: 4, 14, 2

Output: 6

Explanation: In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
showing the four bits relevant in this case). So the answer will be:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.
```

Note:

* Elements of the given array are in the range of 0 to 10^9
* Length of the array will not exceed 10^4.

#### 解法思路 - 爆力法 + Cache \(但會 timeout\)

```text
Time complexity: O(N^2)
```

```cpp
class Solution {
private:
    int hammingDistance(int x, int y, unordered_map<int,int> cache){
        int temp = x^y;
        int origin = temp;
        if(cache.count(temp)){
            return cache[temp];
        }

        int res = 0;
        while(temp != 0){
            res++;
            temp &= (temp - 1);
        }
        cache[origin] = res;
        return res;
    }
public:
    int totalHammingDistance(vector<int>& nums) {
        int res = 0;
        // def: 用來儲放已經算過的 hamming
        unordered_map<int,int> cache;
        for (int i=0; i < nums.size(); i++){
            for (int j=i+1; j < nums.size(); j++){
                res += hammingDistance(nums[i], nums[j], cache);
            }
        }
        return res;
    }
};
```

#### 解法思路 - 分解法

```text
Time Complexity : O(N*32)
Space Complexity : O(1)
```

這題的解法思路為，hamming distance 代表兩個數字的二進位不同的數字，假設為 2\(010\) 與 1\(001\) 那答案為 2。事實上就是第 0 個位置的 1 與 0 的差異數 + 第 1 個位置的差異數。

那這樣假設有第三個數字進行，那他們總共的 hamming distance 也可以該成一個位數一個位數的計算，如下範例。

```text
Input: 4, 14, 2, 5

0100(4)
1110(14)
0010(2)
0101(5)

32 bit
(位置 => 第 i 為 0 的數值 : 第 i 為 1 的數值 => hamming distance)
0 => 4,14,2 : 5 => 3
1 => 4,5 : 14,2 => 4
2 => 2 : 14,4,5 => 3
3 => 4,2,7 : 14 => 3
hameing distance = 13
```

```cpp
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int res = 0;
        int mask = 1;
        for (int i=0; i < 31; ++i){
            int one = 0;
            int zero = 0;
            for (int j=0; j < nums.size(); j++){
                if(nums[j] & mask){
                    one++;
                }else{
                    zero++;
                }
            }
            res += (one * zero);
            mask <<= 1;
        }
        return res;
    }
};
```

