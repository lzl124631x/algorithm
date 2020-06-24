# Kruskal's Algorithm

Kruskal's Algorithm is a minimum-spanning-tree algorithm which finds a minimal spanning tree for a connected weighted graph.

It's a **greedy** algorithm.

It uses **UnionFind** (aka Disjoint Set).

## Algorithm

* create a forest F (a set of trees), where each vertex in the graph is a separate tree
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
    sort(begin(edges), end(edges), [](const vector<int> &a, const vector<int> &b) { return a[2] < b[2]; });
    UnionFind uf(N);
    int cost = 0;
    for (auto &e : edges) {
        int u = e[0], v = e[1], w = e[2];
        if (uf.connected(u, v)) continue;
        uf.connect(u, v);
        cost += w;
        if (uf.getSize() == 1) break;
    }
    return uf.getSize() == 1 ? cost : -1;
}
```

## Reference

* https://en.wikipedia.org/wiki/Kruskal%27s_algorithm
* https://www.luogu.com.cn/problem/P3366