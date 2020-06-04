# Two Sum

[56. Two Sum](https://www.lintcode.com/problem/two-sum/?_from=ladder&&fromId=15)

#### 題目

Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers \(both index1 and index2\) are zero-based.

Example

```text
Example1:
numbers=[2, 7, 11, 15], target=9
return [0, 1]
Example2:
numbers=[15, 2, 7, 11], target=9
return [1, 2]
```

Challenge Either of the following solutions are acceptable:

O\(n\) Space, O\(nlogn\) Time O\(n\) Space, O\(n\) Time

#### 解法 - Map

```cpp
class Solution {
public:
    /**
     * @param numbers: An array of Integer
     * @param target: target = numbers[index1] + numbers[index2]
     * @return: [index1, index2] (index1 < index2)
     */
    vector<int> twoSum(vector<int> &numbers, int target) {
        // write your code here
        std::unordered_map<int, int> map;
        vector<int> res{-1,-1};
        if(numbers.size() == 0){
            return res;
        }

        // 這裡不需要先一個 for loop 來建立 hash 值
        // 因為 [a,b] 在找到 b 時，a 的 hash 值一定是建立好的
        for (int j=0; j < numbers.size(); j++){
            int diff = target - numbers[j];
            if(map.count(diff)){
                res[0] = map[diff];
                res[1] = j;
                break;
            }
            map[numbers[j]] = j;
        }

        return res;
    }
};
```

#### 解法 - 雙指針

注意以下幾個 corner case

* \[0,4,3,0\] =&gt; 有兩個數相同
* \[1,0,-1\] =&gt; 這個沒有依順序

雙指針的解法要求為排序，題目要求回傳位置，只要使用排序就會打亂位置，因此需要使用 map 或啥的來記錄『原位置』。

```cpp
class Solution {
struct Less {
    bool operator()(pair<int,int> &a, pair<int,int> &b){
        return a.second < b.second;
    }
};
public:
    /**
     * @param numbers: An array of Integer
     * @param target: target = numbers[index1] + numbers[index2]
     * @return: [index1, index2] (index1 < index2)
     */
    vector<int> twoSum(vector<int> &numbers, int target) {
        // write your code here

        vector<int> res {-1,-1};
        vector<pair<int,int>> c_numbers;

        for (int i=0; i < numbers.size(); i++){
            c_numbers.push_back(make_pair(i, numbers[i]));
        }

        sort(c_numbers.begin(), c_numbers.end(), Less());

        int start = 0;
        int end = numbers.size() - 1;

        while(start < end){

            if(c_numbers[start].second + c_numbers[end].second == target){
                res[0] = min(c_numbers[start].first, c_numbers[end].first);
                res[1] = max(c_numbers[start].first, c_numbers[end].first);
                break;
            }

            if(c_numbers[start].second + c_numbers[end].second < target){
                start++;
            }else{
                end--;
            }
        }
        return res;
    }
};
```

