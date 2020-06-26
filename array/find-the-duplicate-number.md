---
description: Janu30
---

# Find the Duplicate Number

[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

#### 相似題

* [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
* [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

#### 題目

Given an array nums containing n + 1 integers where each integer is between 1 and n \(inclusive\), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Example 1:

```text
Input: [1,3,4,2,2]
Output: 2
```

```text
Input: [3,1,3,4,2]
Output: 3
```

1. You must not modify the array \(assume the array is read only\).
2. You must use only constant, O\(1\) extra space.
3. Your runtime complexity should be less than O\(n2\).
4. There is only one duplicate number in the array, but it could be repeated more than once.

#### 思路

這一題的解法，最簡單想的是為 sort 後再找有沒有重複的值。

```text
time complexity : O(nlogn)
space complexity : O(1)
```

接下來就是用一個 set 來存，然後當發現某個值 set 中有存在了，那它就是重複數。

```text
time complexity : O(n)
space complexity : O(n)
```

#### 解法 - Binary Search

[Pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle)

題目是給定數字 1 ~ n 個陣列，但總數為 n+1，所以咱們可以這樣想。

如果沒有重複的情況，我們取他的中間數，例如 123456，取到 3，那這個陣數中 :

> 比 3 大的個數與比 3 小等於的個數，應該是會相同的

但是如果有其中一邊比較大，那不就代重複數在大的那裡嗎 ?

所以根據這個思路就可以使用 binary search 來判斷重複的數是在那一邊。

```text
time complexity: O(nlogn)
space complexity: O(1)
```

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int left = 0;
        int right = nums.size();

        while(left < right){
            int mid = (left + right)/2;
            // def: 用來計算小於等於 mid 的總數
            int count = 0;

            for(int i=0; i < nums.size();i++){
                if(nums[i] <= mid){
                    count++;
                }
            }

            if(count <= mid){
                left = mid + 1;
            }else{
                right = mid;
            }
        }
        return right;
    }
};
```

#### 解法 -  Floyd's Tortoise and Hare \(Cycle Detection\) 快慢指針思路

```text
Time complexity: O(N)
Space complexity: O(1)
```

假設我們有以下的陣列 :

```text
[1,3,4,2,2]
```

然後我們將 index 當成 node 節點，然後 index 的 value 當成 next 位置，根據這規則可以產生以下的 link list。

```text
0->1->3->2->4->2
```

然後這樣就可以使用快慢指針，來看看是否有 cycle 如果快慢指針有相交，則有 cycle。

然後接下來，我們要找 cycle 的開始點，如果下紫色箭頭的地方。這裡就需要使用 [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) 的推導，來尋找 cycle 開始點。

推導過程如下，其中 x、y、z 請看下圖，代表步數 :

![](http://yixiang8780.com/outImg/20206026-listcycle.png)

```text
(1) f = 2s (快指針步數 = 2*慢指針步數)
(2) f = x + n(y+z) + y (快指針走到相交點的步數)
(3) s = x + y (慢指針走到相交點的步數)

(4) x + n(y+z) + y = 2(x+y)
(5) x + n(y+z) + y = 2x + 2y
(6) n(y+z) = x+y
(7) x = n(y+z) - y
假設 y+z = k => 快指針走 cycle 一圈的距離

x = n(k) - (k-z)
x = nk - k + z
x = k(n-1) + z (k(n-1) 代表快指針走了 n-1 圈，所以看結果時可以忽略)
```

根據上述的推道，這也代表當從相交點走 z 步，等同於從原點在 x 步，然後這個點就是『 cycle 起始點 』。

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {

        int slow = 0;
        int fast = 0;

        // 相交位置
        while(true){
            slow = nums[slow]; 
            fast = nums[nums[fast]];
            if(slow == fast){
                break;
            }
        }
        slow = 0;
        while(slow != fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
```

