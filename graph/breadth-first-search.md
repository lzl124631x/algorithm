# Breadth First Search (BFS)

## Note

Use `queue<T>` to store the nodes in Breadth-first order.

To avoid visiting the same node twice, use a `vector<bool>` or `unordered_set<T>` to store whether the node has been visited.

Make sure marking the node as visited **before** the node is enqueued! Otherwise the same node might be added multiple times.

## Problems

* [1311. Get Watched Videos by Your Friends (Medium)](https://leetcode.com/problems/get-watched-videos-by-your-friends/)