# Dynamic Programming

## Signal of DP

* The problem wants us to find the number of ways to do something without giving specific steps like how to achieve it. This can be a typical signal that dynamic programming may come to help. [https://leetcode.com/problems/strange-printer/discuss/106810/Java-O\(n3\)-DP-Solution-with-Explanation-and-Simple-Optimization](https://leetcode.com/problems/strange-printer/discuss/106810/Java-O%28n3%29-DP-Solution-with-Explanation-and-Simple-Optimization)
* We can use DPS to solve the problem but the time complexity will be too high. If we find we meet the same subproblem over and over again, it's a signal that we can use DP to solve it because top-down DP is basically just a DFS with memo.

## Basic DP

### Problems

* [691. Stickers to Spell Word](https://leetcode.com/problems/stickers-to-spell-word/)
* [174. Dungeon Game \(Hard\)](https://leetcode.com/problems/dungeon-game/)
* [576. Out of Boundary Paths \(Medium\)](https://leetcode.com/problems/out-of-boundary-paths/)
* [446. Arithmetic Slices II - Subsequence \(Hard\)](https://leetcode.com/problems/arithmetic-slices-ii-subsequence/)

## Interval DP

First type k个连续区间 410

Second Type 375 1246

## Bitmask DP

* [1125. Smallest Sufficient Team \(Hard\)](https://leetcode.com/problems/smallest-sufficient-team/)

## weighted interval scheduling dp

## State Compression DP

Usually used to solve NP-hard problems on small scale dataset, faster then search \(DFS or BFS\).

Example: Travelling salesman problem \(TSP\)

### Problems

* [1349. Maximum Students Taking Exam \(Hard\)](https://leetcode.com/problems/maximum-students-taking-exam/)