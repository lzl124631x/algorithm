# Shortest Path

Algorithm | Time Complexity | Usecase | Memo
---|---|---|---
Bellman-Ford | O(VE) | Single Source to All | Use all the edges to relax `V-1` times
Dijkstra | O(ElogE) with heap.<br/>O(E + VlogV) with Fabonacci Heap | Single Source to All | Heap with closest nodes
Floyd-Warshall | O(V^3) | All to All | Use each node as a intermediate node to update `N^2` pairs of nodes