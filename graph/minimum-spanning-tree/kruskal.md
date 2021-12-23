# Kruskal

Kruskal's Algorithm is a minimum-spanning-tree algorithm which finds a minimal spanning tree for a connected weighted graph. 

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
// Time: O(ElogE)
// Space: O(V)
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

When the run time limitation is tight and it's a dense graph `E ~= V^2`, using `sort` might cause TLE. We need to use `heap` instead, which will reduce the time complexity from `V^2 * log(V^2)` to `K * log(V^2)` where `K` is the number of edges we need to scan to complete the tree. `K` is usually way smaller than `V^2`.

The following solution is for [1584. Min Cost to Connect All Points (Medium)](https://leetcode.com/problems/min-cost-to-connect-all-points/).

```cpp
// OJ: https://leetcode.com/problems/min-cost-to-connect-all-points/
// Author: github.com/lzl124631x
// Time: O(K * log(N^2))
// Space: O(N^2)
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
class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& A) {
        int N = A.size(), ans = 0;
        vector<array<int, 3>> E;
        for (int i = 0; i < N; ++i) {
            for (int j = i + 1; j < N; ++j) E.push_back({ abs(A[i][0] - A[j][0]) + abs(A[i][1] - A[j][1]), i, j });
        }
        make_heap(begin(E), end(E), greater<array<int, 3>>());
        UnionFind uf(N);
        while (uf.getSize() > 1) {
            pop_heap(begin(E), end(E), greater<array<int, 3>>());
            auto [w, u, v] = E.back();
            E.pop_back();
            if (uf.connected(u, v)) continue;
            uf.connect(u, v);
            ans += w;
        } 
        return ans;
    }
};
```

## Problems

* [1135. Connecting Cities With Minimum Cost (Medium)](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)
* [1584. Min Cost to Connect All Points (Medium)](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Reference

* [https://en.wikipedia.org/wiki/Kruskal%27s\_algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm)
* [https://www.luogu.com.cn/problem/P3366](https://www.luogu.com.cn/problem/P3366)

