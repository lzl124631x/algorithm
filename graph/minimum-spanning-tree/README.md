# Minimum Spanning Tree

A minimum-spanning-tree is a tree (without cycles) connecting all the vertices and with the smallest cost.

## Comparison of Kruskal's Algorithm and Prim's Algorithm 

Prim's (`O(ElogV)`) is more performant on dense graph. Kruskal's (`O(ElogE)`) is more performant on sparse graph.

| Aspect                   | Prim's Algorithm              | Kruskal's Algorithm           |
|--------------------------|-------------------------------|--------------------------------|
| **Algorithm Type**       | Greedy                        | Greedy                         |
| **Edge Selection**       | Chooses the minimum-weight edge connected to the current MST. | Sorts all edges by weight and adds them to the MST if they don't form a cycle. |
| **Data Structures Used** | Priority queue or min-heap (to efficiently select the minimum-weight edge). | Disjoint-set data structure (to check for cycles). |
| **Complexity**           | O(V^2) using an adjacency matrix, O(E + V log V) using a binary heap for dense graphs. | O(E log E) to sort the edges, O(E log V) to process all edges and find MST using disjoint-set. |
| **Suitability**          | More efficient for dense graphs (E is close to V^2). | Suitable for sparse graphs (E is much less than V^2). |
| **Applications**         | Network design, clustering, road planning, maze generation, etc. | Network design, cluster analysis, image segmentation, etc. |
| **Cycle Handling**       | Doesn't directly deal with cycles; relies on the connectedness of the graph. | Detects and avoids cycles explicitly. |
| **Output**               | Yields a single MST rooted at a selected vertex. | Yields a forest of MSTs initially; they combine into a single MST as more edges are added. |

## Problems

* [1584. Min Cost to Connect All Points (Medium)](https://leetcode.com/problems/min-cost-to-connect-all-points/) Dense Graph. Prim is better.
* [1489. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree (Hard)](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/)