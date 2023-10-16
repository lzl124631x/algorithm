# Interval Scheduling Maximization (ISM)

The interval scheduling maximization (ISM) problem is to find a largest compatible set - a set of non-overlapping intervals of maximum size. The goal here is to execute as many tasks as possible.

## Algorithm

Greedily select the interval with the **earliest ending time**.

1. Sort the intervals in ascending order of the **end time**. (We don't care the start time)
2. Scan through the intervals and greedily collect non-overlapping intervals.

Memorize point 1: For all the intervals whose start times are not overlapping with earlier intervals, picking the one ending first must be optimal.

## Implementation

```cpp
// OJ: https://leetcode.com/problems/non-overlapping-intervals/
// Author: github.com/lzl124631x
// Time: O(NlogN)
// Space: O(1)
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& A) {
        sort(begin(A), end(A), [](vector<int> &a, vector<int> &b) { return a[1] < b[1]; });
        int end = INT_MIN, ans = 0; // `end` is the maximum ending time of selected intervals
        for (auto &x : A) {
            if (x[0] >= end) end = x[1]; // this interval doesn't have overlap with the previously selected interval, select it and update the `end`.
            else ++ans; // otherwise, overlapped intervals are skipped.
        }
        return ans;
    }
};
```

## Problem

* [435. Non-overlapping Intervals (Medium)](https://leetcode.com/problems/non-overlapping-intervals/)
* [452. Minimum Number of Arrows to Burst Balloons (Medium)](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)
* [646. Maximum Length of Pair Chain (Medium)](https://leetcode.com/problems/maximum-length-of-pair-chain/) **Direct Application**
* [1520. Maximum Number of Non-Overlapping Substrings (Medium)](https://leetcode.com/problems/maximum-number-of-non-overlapping-substrings/)

## Reference

* https://en.wikipedia.org/wiki/Interval_scheduling
