---
description: 注意、基本排序、Amazon
---

# Sort Integers II

[464. Sort Integers II](https://www.lintcode.com/problem/sort-integers-ii/?_from=ladder&&fromId=78)

#### 題目

Given an integer array, sort it in ascending order in place. Use quick sort, merge sort, heap sort or any O\(nlogn\) algorithm.

Example Example1:

```text
Input: [3, 2, 1, 4, 5], 
Output: [1, 2, 3, 4, 5].
```

#### 解法 - Quick Sort

[拿鐵派-排序之快速排序法\(Quick Sort\)](https://mark-lin.com/posts/20170425/)

```cpp
class Solution {
private:
    void swap(vector<int> &nums, int a , int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
    void quickSort(vector<int> &nums, int start, int end){
        if(start >= end){
            return;
        }

        int left = start;
        int right = end;
        int privot = nums[(left+right)/2];

        while(left <= right){

            while(nums[left] < privot){
                left++;
            }

            while(nums[right] > privot){
                right--;
            }

            if(left <= right){
                swap(nums, left,right);
                left++;
                right--;
            }
        }

        quickSort(nums, left, end);
        quickSort(nums, start, right);
    }
public:
    /**
     * @param A: an integer array
     * @return: nothing
     */
    void sortIntegers2(vector<int> &A) {
        // write your code here
        quickSort(A, 0, A.size() - 1);
    }
};
```

#### 解法 - Merge Sort

#### 解法 - Heap Sort

