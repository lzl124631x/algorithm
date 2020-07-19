# Binary Lifting

This technique is based on the fact that every integer can be represented in binary form.

Through pre-processing, a sparse table `dp[i][v]` can be calculated which stores the `2^i`th parent of vertex `v`, where `0 <= i <= logN`. (Example: [1483. Kth Ancestor of a Tree Node \(Hard\)](https://leetcode.com/problems/kth-ancestor-of-a-tree-node/)) This pre-processing takes `O(NlogN)` time.

When we want to get the `k`th parent of `v`, we can first break `k` into the binary representation `k = 2^p1 + 2^p2 + 2^p3 + ... + 2^pj` where `p1, ..., pj` are the indices where `k` has bit value `1`.

So we can get the `2^p1`th, `2^p2`th, ... `2^pj`th parent in any order to get `k`th parent of `v`.

Example: to get the `6`th parent of `v`, since `6 = 2^1 + 2^2`, the answer can be `P[1][ P[2][v] ]` or `P[2][ P[1][v] ]`.

Hence we can reduce the time complexity of getting the `K`th parent from `O(K)` to `O(logK)`.

## Problems

* [1483. Kth Ancestor of a Tree Node \(Hard\)](https://leetcode.com/problems/kth-ancestor-of-a-tree-node/)

