# K Subset Partitioning

K Subset partitioning: partition the original array into `K` subsets and find the optimial result.

Candidate Solutions:

1. DFS + Optimizations
2. DP on Subsets


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

### DP on Subsets

For bit manipulation related to subsets, see the section "Bit Manipulation".

