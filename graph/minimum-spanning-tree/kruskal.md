# Kruskal

Kruskal's Algorithm is a minimum-spanning-tree algorithm which finds a minimal spanning tree for a connected weighted graph. A minimum-spanning-tree is a tree (without cycles) connecting all the vertices and with the smallest cost.

It's a **greedy** algorithm.

It uses **UnionFind** \(aka Disjoint Set\).

## Algorithm

**Summary: Try to include the edges one by one from smallest cost to greatest cost. Skip those causing cycles. Stop once all nodes are connected.**

* create a forest F \(a set of trees\), where each vertex in the graph is a separate tree
* create a set S containing all the edges in the graph
* while S is nonempty and F is not yet spanning
  * remove an edge with minimum weight from S
  * if the removed edge connects two different trees then add it to the forest F, combining two trees into a single tree

At the termination of the algorithm, the forest forms a minimum spanning forest of the graph. If the graph is connected, the forest has a single component and forms a minimum spanning tree

## Implementation

```cpp
class UnionFind {
    vector<int> id;
    int size;
public:
    UnionFind(int N) : id(N), size(N) {
        iota(begin(id), end(id), 0);
    }
    int find(int a) {
        return id[a] == a ? a : (id[a] = find(id[a]));
    }
    void connect(int a, int b) {
        int p = find(a), q = find(b);
        if (p == q) return;
        id[p] = q;
        --size;
    }
    bool connected(int a, int b) {
        return find(a) == find(b);
    }
    int getSize() { return size; }
};

int kruskal(int N, vector<vector<int>> edges) {
    sort(begin(edges), end(edges), [](const vector<int> &a, const vector<int> &b) { return a[2] < b[2]; }); // Sort the edges in ascending order of weight.
    UnionFind uf(N);
    int cost = 0;
    for (auto &e : edges) { // try the edges from smallest weight to greatest weight
        int u = e[0], v = e[1], w = e[2];
        if (uf.connected(u, v)) continue; // If this edge will cause cycle, skip it.
        uf.connect(u, v); // otherwise, pick this edge, connect its vertices.
        cost += w; // add its weight to the minimum-spanning-tree's cost
        if (uf.getSize() == 1) break; // Once all nodes are connected, terminate.
    }
    return uf.getSize() == 1 ? cost : -1; // If all nodes are connected, return the cost; otherwise, this graph doesn't have minimum-spanning-tree.
}
```

## Reference

* [https://en.wikipedia.org/wiki/Kruskal%27s\_algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm)
* [https://www.luogu.com.cn/problem/P3366](https://www.luogu.com.cn/problem/P3366)

