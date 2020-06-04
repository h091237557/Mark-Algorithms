# Closest Target Value

[1478. Closest Target Value](https://www.lintcode.com/problem/closest-target-value/?_from=ladder&&fromId=59)

#### 題目

Given an array, find two numbers in the array and their sum is closest to the target value but does not exceed the target value, return their sum.

Example Example1

```text
Input: target = 15 and array = [1,3,5,11,7]
Output: 14
Explanation: 
11+3=14
```

Example2

```text
Input: target = 16 and array = [1,3,5,11,7]
Output: 16
Explanation: 
11+5=16
```

Notice if there is no result meet the requirement, return -1.

#### 解法 - 雙指針

需注意的 corner case 有 :

* 全部都是負的情況

```cpp
class Solution {
public:
    /**
     * @param target: the target
     * @param array: an array
     * @return: the closest value
     */
    int closestTargetValue(int target, vector<int> &array) {
        // Write your code here
        int left = 0;
        int right = array.size() - 1;

        sort(array.begin(), array.end());

        int res = INT_MIN;
        while(left  < right){
            int sum = array[left] + array[right];
            if(sum == target){
                return target;
            }

            if(sum < target){
                res = max(res, sum);
                left++;
            }else{
                right--;
            }
        }

        if(res == INT_MIN){
            return -1;
        }

        return res;
    }
};
```

