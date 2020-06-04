# Second Max of Array

[479. Second Max of Array](https://www.lintcode.com/problem/second-max-of-array/?_from=ladder&&fromId=59)

#### 題目

Find the second max number in a given array.

Example Example1:

```text
Input: [1,3,2,4]
Output: 3
```

Example2:

```text
Input: [1,1,2,2]
Output: 2
```

#### 解法 - Heap

這題是要求說，給定一個陣列，找出第二大的數。最簡單的解法一定是排序找抓第二大的數，但這個時間複雜度要 o\(nlogn\)，那接下來的解法就是往 o\(n\) 想。

這裡想到最簡單的解法，使用一個 priory queue，大小為 2。然後就一個一個放進去。最後留下的就是前兩個。

時間複雜度: O\(n\) + O\(logn\) = O\(n\) 空間複雜度: O\(k\)

```cpp
priority_queue<Type, Container, Compare>

push: O(logn + (container 的 push_back ))

備註: push_back 在 vector 為 O(1)
```

```cpp
class Solution {
private:
    struct compare {
        bool operator()(int a, int b){
            return b < a;
        }
    };
public:
    /**
     * @param nums: An integer array
     * @return: The second max number in the array.
     */
    int secondMax(vector<int> &nums) {
        // write your code here
        int k = 2;
        priority_queue<int, vector<int>, compare> pq;

        for (int i=0; i < nums.size(); i++){
            pq.push(nums[i]);

            if(pq.size() > k){
                pq.pop();
            }
        }

        int res = pq.top();
        return res;
    }
};
```

### 解法 - 就兩個數

```cpp
class Solution {
public:
    /**
     * @param nums: An integer array
     * @return: The second max number in the array.
     */
    int secondMax(vector<int> &nums) {
        // write your code here
        int max1 = max(nums[0], nums[1]);
        int max2 = min(nums[0], nums[1]);

        for (int i=2; i < nums.size(); i++){
            if(nums[i] > max1){
                // 大於 max1 那他一定是最大的
                max2 = max1;
                max1 = nums[i];
            }else if(nums[i] > max2){
                max2 = nums[i];
            }
        }
        return max2;
    }
};
```

