# Dynamic Programming

Dynamic Programming is basically DFS with memoization.

We can use DFS to solve the DP problem in small scale, but when the input size is large, plain DFS will be too slow because we recompute the result for some specific states over and over again, wasting time.

## What's the difference between Top-down DP and Bottom-up DP?

Top-down DP:

* = DFS + memo
* It's recursive
* We start from the original problem, and search into sub-problems \(i.e. use the results of the sub-problems\).
* If we visualize the DFS search as a tree, top-down DP is going from the root to leaf nodes.

Bottom-up DP:

* It's iterative
* We start from resolving the minimal sub-problems, then use those results to build up solutions for sub-problems with larger scale, until we get enough results to build the solution for the original problem.
* If we visualize the DFS search as a tree, bottom-up DP is going from the leaf nodes to the root.

## When to use DP?

1. it involves searching, especially DFS. We can use DFS to solve the problem in small scale
2. it only cares about the optimal solution -- Optimal substructure
3. During searching, we might go into the same state from different paths -- Overlapping sub-problems.

## What is the common process to solve DP problems?

1. Think in DFS way. Identify the states we need to use in each DFS invocation.
2. Define the transition between states.
3. Define special/trivial cases.

## Types of DP

### Sequence DP

We scan the sequence one by one, make decisions at each step.

#### Problems

* [691. Stickers to Spell Word](https://leetcode.com/problems/stickers-to-spell-word/)
* [174. Dungeon Game \(Hard\)](https://leetcode.com/problems/dungeon-game/)
* [576. Out of Boundary Paths \(Medium\)](https://leetcode.com/problems/out-of-boundary-paths/)
* [446. Arithmetic Slices II - Subsequence \(Hard\)](https://leetcode.com/problems/arithmetic-slices-ii-subsequence/)
* [1463. Cherry Pickup II \(Hard\)](https://leetcode.com/problems/cherry-pickup-ii/)
* [1473. Paint House III \(Hard\)](https://leetcode.com/problems/paint-house-iii/)
* [44. Wildcard Matching \(Hard\)](https://leetcode.com/problems/wildcard-matching/)

### Interval DP

* [1478. Allocate Mailboxes \(Hard\)](https://leetcode.com/problems/allocate-mailboxes/)
* [410. Split Array Largest Sum \(Hard\)](https://leetcode.com/problems/split-array-largest-sum/)
* [1140. Stone Game II (Medium)](https://leetcode.com/problems/stone-game-ii/)

Second Type 375 1246

### weighted interval scheduling dp

### State Compression DP

Usually used to solve NP-hard problems on small scale dataset, faster then search \(DFS or BFS\).

Example: Travelling salesman problem \(TSP\)

#### Problems

* [1349. Maximum Students Taking Exam \(Hard\)](https://leetcode.com/problems/maximum-students-taking-exam/)

### Uncategoried

* [1000. Minimum Cost to Merge Stones \(Hard\)](https://leetcode.com/problems/minimum-cost-to-merge-stones/)