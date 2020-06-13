---
description: 九章
---

# Insert Delete GetRandom O\(1\) - Duplicates allowed

[954. Insert Delete GetRandom O\(1\) - Duplicates allowed](https://www.lintcode.com/problem/insert-delete-getrandom-o1-duplicates-allowed/description)

#### 題目

Design a data structure that supports all following operations in average O\(1\) time.

Example Example 1:

```text
Input:
insert(1)
insert(1)
insert(2)
getRandom()
remove(1)

// Init an empty collection.
RandomizedCollection collection = new RandomizedCollection();

// Inserts 1 to the collection. Returns true as the collection did not contain 1.
collection.insert(1);

// Inserts another 1 to the collection. Returns false as the collection contained 1. Collection now contains [1,1].
collection.insert(1);

// Inserts 2 to the collection, returns true. Collection now contains [1,1,2].
collection.insert(2);

// getRandom should return 1 with the probability 2/3, and returns 2 with the probability 1/3.
collection.getRandom();

// Removes 1 from the collection, returns true. Collection now contains [1,2].
collection.remove(1);

// getRandom should return 1 and 2 both equally likely.
collection.getRandom();
```

Notice Duplicate elements are allowed.

* insert\(val\): Inserts an item val to the collection.
* remove\(val\): Removes an item val from the collection if present.
* getRandom: Returns a random element from current collection of elements. The probability of each element being returned is linearly related to the number of same value the collection contains.

#### 思路

這一題與前一題相比，多了一個條件 :

> Duplicate elements are allowed.

前一題我們使用了 array 與 hashmap 來將 insert、remove、getRandom 達成 o\(1\) 的時間複雜度，接下來咱們來盤一下，如果加了 Duplicate 這條件，他們會有什麼改變。

* insert : array 處理沒改變，但新增到 hashmap 會有影響，重複的會被蓋掉。
* remove : 如果有兩個 1、1 則移除 1 之後移除其中一個。

> 那這題的要點在於，在儲 hashmap 時，要如何儲放重複的。

方案 : hashmap + queue

map 

insert\(val\) : 新增至 array，且將 val 當 map key，然後丟入到 map 的 queue。 remove\(val\) : 這裡分成兩條分支。如果 queue 只剩一個則 erase map，然後將使用題目 1 的移除 array 法。如果 queue 有多個，則從 queue 中 pop 出一個，然後在用題目 1 的移除法。

```cpp
class RandomizedCollection {
private:
    unordered_map<int,std::queue<int>> map;
    vector<int> nums;
    int size;
public:
    /** Initialize your data structure here. */
    RandomizedCollection() {
        size = 0;
    }

    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    bool insert(int val) {
        // write your code here
        map[val].push(size);
        nums.push_back(val);
        size++;
        return true;
    }

    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    bool remove(int val) {
        // write your code here
        if(!map.count(val)){
            return false;
        }

        int index = map[val].front();
        int last = size - 1;
        // remove from array
        nums[index] = nums[last];
        nums.pop_back();
        // remove from map
        map[nums[last]].pop();
        map[nums[last]].push(index);
        if(map[val].size() == 1){
            map.erase(val);
        }else{
            map[val].pop();
        }
        size--;
        return true;
    }

    /** Get a random element from the collection. */
    int getRandom() {
        // write your code here
        return nums[rand() % size];
    }
};

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * RandomizedCollection obj = new RandomizedCollection();
 * bool param_1 = obj.insert(val);
 * bool param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

