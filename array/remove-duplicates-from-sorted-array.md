# Remove Duplicates from Sorted Array

[100. Remove Duplicates from Sorted Array](https://www.lintcode.com/problem/remove-duplicates-from-sorted-array/?_from=ladder&&fromId=116)

#### 題目

Given a sorted array, 'remove' the duplicates in place such that each element appear only once and return the 'new' length.

Do not allocate extra space for another array, you must do this in place with constant memory.

Example Example 1:

```text
Input:  []
Output: 0
```

Example 2:

```text
Input:  [1,1,2]
Output: 2    
Explanation:  uniqued array: [1,2]
```

#### 解法

```cpp
class Solution {
public:
    /*
     * @param nums: An ineger array
     * @return: An integer
     */
    int removeDuplicates(vector<int> &nums) {
        // write your code here
        if( nums.size() == 0 ){
            return 0; 
        }

        int res = 1;
        int start = 1;

        for (int i=1; i < nums.size(); i++){
            if(nums[i] != nums[i-1]){
                res++;
                nums[start] = nums[i];
                start++;
            }
        }
        return res;
    }
};
```

