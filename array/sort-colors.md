---
description: 九章、June30
---

# Sort Colors

[148. Sort Colors](https://www.lintcode.com/problem/sort-colors/description)

#### 題目

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Example Example 1

```text
Input : [1, 0, 1, 2]
Output : [0, 1, 1, 2]
Explanation : sort it in-place
```

Challenge A rather straight forward solution is a two-pass algorithm using counting sort. First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.

Could you come up with an one-pass algorithm using only constant space?

Notice You are not suppose to use the library's sort function for this problem. You should do it in-place \(sort numbers in the original array\).

#### 解法 - Quick Sort

> 注意 while 下面的兩個 &gt; 與 &lt; 和 &gt;= 與 &lt;= 的差別

```cpp
class Solution {
private:
    void swap(vector<int>& nums, int a, int b){
        int tmep = nums[a];
        nums[a] = nums[b];
        nums[b] = tmep;
    }
    void quickSort(vector<int>& nums, int start, int end){
        if(start >= end){
            return;
        }

        int left = start;
        int right = end;
        int privot = nums[left];

        while(left <= right){
            // find the >= privot target
            while(left <= right && nums[left] < privot){
                left++;
            }
            // find the <= privot target
            while(left <= right && nums[right] > privot){
                right--;
            }

            if(left <= right){
                swap(nums, left, right);
                left++;
                right--;
            }
        }

        quickSort(nums, start, right);
        quickSort(nums, left, end);
    }
public:
    /**
     * @param nums: A list of integer which is 0, 1 or 2 
     * @return: nothing
     */
    void sortColors(vector<int> &nums) {
        // write your code here
        quickSort(nums,0, nums.size()-1);
    }
};
```

