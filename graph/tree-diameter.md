# Tree Diameter

## DFS Twice

* Pick a random node `i`.
* DFS to find the furthest node `j` to node `i`.
* DFS to find the furthest node `k` to node `j`.

The distance between `j` and `k` is the tree's diameter.

See [proof](https://oi-wiki.org/graph/tree-diameter/)

## Problems

* [1617. Count Subtrees With Max Distance Between Cities (Hard)](https://leetcode.com/problems/count-subtrees-with-max-distance-between-cities/)