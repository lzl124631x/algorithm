# K Subset Partitioning

K Subset partitioning: partition the original array into `K` subsets and find the optimial result.

Candidate Solutions:

1. DFS + Optimizations
2. DP


## Problems

* [698. Partition to K Equal Sum Subsets (Medium)](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)
* [473. Matchsticks to Square (Medium)](https://leetcode.com/problems/matchsticks-to-square/)
* [1723. Find Minimum Time to Finish All Jobs (Hard)](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/)

### DFS + Optimizations

**Algorithm**

Create a `vector` of length `K` to store the subset values.

DFS to visit elements in the input array `A` one by one. In each DFS call, we traverse the `K` subsets and try to add `A[i]` to a subset.

**Tricks**

A important trick is to prevent visiting the same subset value again using `unordered_set`.

For example, if you get 4 subsets and each with sum `10`, and now you want to add `5` to one of them. Plain DFS will try adding `5` to each of these `10`s, but adding to which `10` actually makes no difference.

So we add the visited values into a `unordered_set` and skip visiting the same subset value when we see them again.

Another trick is sorting the `A`. Pick the order that can get a feasible partition as fast as possible.

### DP

Traverse all the subsets: `for (int mask = 0; mask < (1 << N); ++mask)`

Check if `A[i]` is included in the current subset: `mask & (1 << i)`

Traverse subsets of a set `mask` in **descending** order of subset size: 

```cpp
for (int sub = mask; sub; sub = (sub - 1) & mask) {
    // visit subset `sub`
}
```

Traverse subsets of a set `mask` in **ascending** order of subset size:

```cpp
for (int other = mask; other; other = (other - 1) & mask) {
    int sub = other ^ mask;
    // visit subset `sub`
}
```

Given `N` elements, traverse subsets of size `K` (Gosper's Hack):

```cpp
int sub = (1 << k) - 1;            
while (sub < (1 << N)) {
    // visit subset `sub`
    int c = sub & - sub;
    int r = sub + c;
    sub = (((r ^ sub) >> 2) / c) | r;
}
```

Traverse all the subsets of `N` elements, and for each subset `mask`, traverse its sub-subset `sub`:

```cpp
// Time: O(3^N)
for (int mask = 0; mask < (1 << N); ++mask) {
    for (int sub = mask; sub; sub = (sub - 1) & mask) {
        // visit `sub` ans `mask`
    }
}
```

Note that the time complexity is `O(3^N)` instead of `O(2^N * 2^N)`.

**Math proof:**

For a subset `mask` with `K` bits of `1`s, there are `2^K` subsets of it.

For `N` elements, there are `C(N, K)` subsets with `K` bits of `1`s.

So the total number is `SUM( C(N, K) * 2^K | 0 <= K <= N )`.

Since `(1 + x)^N = C(N, 0) * x^0 + C(N, 1) * x^1 + ... + C(N, N) * x^N`, let `x = 2`, we have `3^N = SUM( C(N, K) * 2^K | 0 <= K <= N )`.

**Intuitive proof:**

For each bit index `i` (`0 <= i < N`), the `i`-th bits of `mask` and `bit` must be one of `11, 10, 00`, and can't be `01`. So each bit has `3` possibilities, the total complexity is `O(3^N)`.