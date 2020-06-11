---
description: 九章
---

# Sort Colors II

[143. Sort Colors II](https://www.lintcode.com/problem/sort-colors-ii/description)

#### 題目

Given an array of n objects with k different colors \(numbered from 1 to k\), sort them so that objects of the same color are adjacent, with the colors in the order 1, 2, ... k.

Example Example1

```text
Input: 
[3,2,2,1,4] 
4
Output: 
[1,2,2,3,4]

Input: 
[2,1,1,2,2] 
2
Output: 
[1,1,2,2,2]
```

Challenge A rather straight forward solution is a two-pass algorithm using counting sort. That will cost O\(k\) extra memory. Can you do it without using extra memory?

Notice

* You are not suppose to use the library's sort function for this problem.
* k &lt;= n

#### 解法 - Quick Sort

```cpp
class Solution {
private:
    void swap(vector<int>& nums, int a, int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
    void quickSort(vector<int>& nums, int start, int end){
        if(start >= end){
            return;
        }

        int left = start;
        int right = end;
        // tip: 如果是 5,4,3,2,1 這種順序，privot 選首的會比較慢
        int privot = nums[start];

        while(left <= right){
            while(left <= right && nums[left] < privot){
                left++;
            }
            while(left <= right && nums[right] > privot){
                right--;
            }

            if(left <= right){
                swap(nums, left, right);
                left++;
                right--;
            }
        }

        quickSort(nums,start, right);
        quickSort(nums,left, end);
    }
public:
    /**
     * @param colors: A list of integer
     * @param k: An integer
     * @return: nothing
     */
    void sortColors2(vector<int> &colors, int k) {
        // write your code here
        quickSort(colors, 0, colors.size()-1);
    }
};
```

#### 解法 - 計數排序 \(O^2\)

```cpp
class Solution {
public:
    /**
     * @param colors: A list of integer
     * @param k: An integer
     * @return: nothing
     */
    void sortColors2(vector<int> &colors, int k) {
        // write your code here
        vector<int> count(k+1, 0);

        for (int i=0; i < colors.size(); i++){
            count[colors[i]]++;
        }

        int index = 0;
        for (int j=1; j <= k; j++){
            for (int l=0; l < count[j]; l++){
                colors[index++] = j;
            }
        }
    }
};
```

