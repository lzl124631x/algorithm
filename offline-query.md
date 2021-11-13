# Offline Query

A common trick for solving problems related to a series of queries. 

When we know all the queries upfront, we can sort the query and the original data in some order so that we can scan the query and the original data simultaneously (which takes `O(N + Q)` time), instead of scanning all original data for each query (which takes `O(N * Q)` time).

The name "offline" signifies the fact that we know all the queries upfront/offline. If the queries keep streaming in (i.e. online), we can't reorder the queries to get a better time complexity.

## Problems

* [1707. Maximum XOR With an Element From Array (Hard)](https://leetcode.com/problems/maximum-xor-with-an-element-from-array/)
* [1847. Closest Room (Hard)](https://leetcode.com/problems/closest-room/)
* [2070. Most Beautiful Item for Each Query (Medium)](https://leetcode.com/problems/most-beautiful-item-for-each-query/)

## Discuss

https://leetcode.com/problems/most-beautiful-item-for-each-query/discuss/1576065/C%2B%2B-Offline-Query