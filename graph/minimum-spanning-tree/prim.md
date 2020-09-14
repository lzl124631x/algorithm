# Prim's Algorithm

Prim's algorithm is a **greedy** algorithm that finds a minimum spanning tree for a **weighted undirected** graph.

## Algorithm

**Summary: Starting from a random node, pick its minimum outbound edge that doesn't form cicle, and add the node to the tree**

* Initialize the tree with a random single node.
* Grow the tree by one edge: from the edges that connect the tree to nodes not yet in the tree, find the minimum-weight edge, and add it to the tree.
* Repeat until all nodes are in the tree.

## Implementation



## Problems

* [1584. Min Cost to Connect All Points (Medium)](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Reference

* https://en.wikipedia.org/wiki/Prim%27s_algorithm