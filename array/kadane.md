# Kadane

Kadane's algorithm is used to find the maximum sum subarray from a given array. It's a DP algorithm.

## Algorithm

```cpp
// Time: O(N)
// Space: O(1)
int kadane(vector<int> &A) {
    int ans = INT_MIN, mx = 0;
    for (int n : A) {
        mx = max(mx + n, n);
        ans = max(ans, mx);
    }
    return ans;
}
```

## Problems

* [363. Max Sum of Rectangle No Larger Than K (Hard)](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/solution/)