---
description: 九章、Jane30
---

# Insert Delete GetRandom O\(1\)

[657. Insert Delete GetRandom O\(1\)](https://www.lintcode.com/problem/insert-delete-getrandom-o1/description)

#### 題目

Design a data structure that supports all following operations in average O\(1\) time.

* insert\(val\): Inserts an item val to the set if not already present.
* remove\(val\): Removes an item val from the set if present.
* getRandom: Returns a random element from current set of elements. Each element must have the same probability of being returned.

**題目解說**

設計一個資料結構，並且提供 insert、remove、getRandom 三種操作，它們的時間複雜度要求都為 O\(1\)

Example:

```text
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

#### 思路

**1. 先尋找資料結構**

題目要求 insert 與 remove 都是 O\(1\)，那基本上只剩下 hash 可以選擇。

備註: stack =&gt; insert o\(1\), remove target o\(n\) queue =&gt; insert o\(1\), remove target o\(n\) array =&gt; insert o\(1\), remove target o\(n\) list =&gt; insert o\(1\), remove target o\(n\)

但是 hash 這時如果要 rand 那就無法達到 O\(1\)

```text
ex.

{1,10} .. size = 2

無法使用 2 來 random 出某個值。
```

因為要完成 rand O\(1\) 的時間複雜度，那需要使用以下兩個資料結構來組合 :

* hash
* array

hash =&gt; key 為對應的值，value 儲放連結的 array 位置 array =&gt; 儲放值

Ex.

insert\(1\) insert\(10\)

map = { 1 =&gt; 0, 10 =&gt; 1 }

array = \[1, 10\];

這樣要 random 時，就可以達成 O\(1\) 的時間複雜度。

**remove 的問題**

如果只有 hash 那 remove 要達成 o\(1\) 可行，但是多了 array，它要進行 remove 就會是 o\(n\)，因為要從頭找到目標。

因此這裡的處理方法就是進行三步操作 :

1. 將 remove 目標 swap 至最後一個位置 \( O\(1\) \)
2. 從 array 中移除最後一個位置 \( O\(1\) \)
3. 更新 map 位置，將原本 last 連結位置，改成 remove 的位置

**Code**

```text
Runtime: 48 ms, faster than 98.65% of C++ online submissions for Insert Delete GetRandom O(1).
Memory Usage: 22.3 MB, less than 27.08% of C++ online submissions for Insert Delete GetRandom O(1).
```

```cpp
class RandomizedSet {
private:
    unordered_map<int, int> map;
    vector<int> datas;
    int size_;
public:
    /** Initialize your data structure here. */
    RandomizedSet() {
        size_ = 0;
    }

    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        if(map.count(val)){
            return false;
        }

        datas.push_back(val);
        // 將 val 丟到 hash 中，並且 hash 的 value 要存放放在陣列的位置
        map[val] = size_;
        size_++;

        return true;
    }

    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {

        if(!map.count(val)){
            return false;
        }
        // 從 map 中找出 val 的 datas 位置在從此處將它移除
        int index = map[val];

        // vector 刪除不是 o(1)，因此需要進行以下的操作
        // swap 目標和最後一個數 o(1)
        // 在刪除 vector 最後一個數 o(1)
        int last = datas[size_ - 1];
        map[last] = map[val];
        datas[index] = last;
        datas.pop_back();
        map.erase(val);

        size_--;

        return true;
    }

    /** Get a random element from the set. */
    int getRandom() {
        return datas[rand() % size_];
    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

