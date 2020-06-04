---
description: Microsoft
---

# Merge Intervals

[156. Merge Intervals](https://www.lintcode.com/problem/merge-intervals/?_from=ladder&&fromId=21)

#### 題目

Given a collection of intervals, merge all overlapping intervals.

Example Example 1:

```text
Input: [(1,3)]
Output: [(1,3)]
```

Example 2:

```text
Input:  [(1,3),(2,6),(8,10),(15,18)]
Output: [(1,6),(8,10),(15,18)]
```

Challenge O\(n log n\) time and O\(1\) extra space.

#### 解法

```cpp
/**
 * Definition of Interval:
 * class Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
private:
    struct Compare {
        bool operator()(Interval a, Interval b){
            return a.start < b.start;
        }
    };
    Interval mergeInterval(Interval a, Interval b){
        Interval res;
        res.start = a.start;
        res.end = (b.end > a.end) ? b.end : a.end;
        return res;
    }
public:
    /**
     * @param intervals: interval list.
     * @return: A new interval list.
     */
    vector<Interval> merge(vector<Interval> &intervals) {
        // write your code here
        vector<Interval> res;
        if(intervals.size() == 0){
            return res;
        }

        if(intervals.size() == 1){
            res.push_back(intervals[0]);
            return res;
        }

        sort(intervals.begin(), intervals.end(), Compare());
        Interval preInterval = intervals[0];
        for (int i=1; i < intervals.size(); i++){
            if(intervals[i].start > preInterval.end){
                res.push_back(preInterval);
                preInterval = intervals[i];
            }else{
                Interval mergedInterval = mergeInterval(preInterval, intervals[i]);
                preInterval = mergedInterval;
            }

            if( i == intervals.size() - 1 ){
                res.push_back(preInterval);
            }
        }

        return res;
    }
};
```

```cpp
/**
 * Definition of Interval:
 * class Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
private:
    struct Compare {
        bool operator()(Interval a, Interval b){
            return a.start < b.start;
        }
    };
public:
    /**
     * @param intervals: interval list.
     * @return: A new interval list.
     */
    vector<Interval> merge(vector<Interval> &intervals) {
        // write your code here
        vector<Interval> res;
        if(intervals.size() == 0){
            return res;
        }

        sort(intervals.begin(), intervals.end(), Compare());
        res.push_back(intervals[0]);
        for (int i=1; i < intervals.size(); i++){
            if(intervals[i].start <= res.back().end){
                res.back().end = max(res.back().end, intervals[i].end);
            }else{
                res.push_back(intervals[i]);
            }
        }

        return res;
    }
};
```

