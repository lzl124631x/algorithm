# Prim's Algorithm

Prim's algorithm is a **greedy** algorithm that finds a minimum spanning tree for a **weighted undirected** graph.

## Algorithm

**Summary: Starting from a random node, pick its minimum outbound edge that doesn't form cicle, and add the node to the tree**

* Initialize the tree with a random single node.
* Grow the tree by one edge: from the edges that connect the tree to nodes not yet in the tree, find the minimum-weight edge, and add it to the tree.
* Repeat until all nodes are in the tree.

## Implementation

Basic implementation without `heap`.

```cpp
// Time: O(V^2)
// Space: O(V)
int prim(int N, int M, vector<vector<int>> &edges) { // N nodes, M edges. edges is an array of `<u, v, w>` where u, v are from and to nodes, w is the weight.
    vector<vector<pair<int, int>>> G(N);
    for (auto &e : edges) {
        int u = e[0] - 1, v = e[1] - 1, w = e[2];
        G[u].emplace_back(v, w);
        G[v].emplace_back(u, w);
    }
    int ans = 0, cur = 0; // start from a random node, say 0.
    vector<int> dist(N, INT_MAX), seen(N);
    for (int i = 0; i < N - 1; ++i) {
        seen[cur] = 1; // add this node to the MST
        for (auto &[v, w] : G[cur]) {
            if (seen[v]) continue; // skip those neighbors that are already in the tree
            dist[v] = min(dist[v], w); // Use the current node to update the distance of unvisited nodes.
        }
        cur = min_element(begin(dist), end(dist)) - begin(dist); // The node with the minimum distance is selected.
        ans += dist[cur];
        dist[cur] = INT_MAX;
    }
    return ans;
}
```

## Reference

* https://en.wikipedia.org/wiki/Prim%27s_algorithm