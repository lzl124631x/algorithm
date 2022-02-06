# Union Find

## Implementation

### Basic

Note that there is a path compression in the `find` function. 

The time complexity of one operation in `UnionFind` with `N` elements and path compression only is amortized `O(logN)`.

```cpp
class UnionFind {
    vector<int> id;
public:
    UnionFind(int n) : id(n) {
        iota(begin(id), end(id), 0);
    }
    int find(int a) {
        return id[a] == a ? a : (id[a] = find(id[a]));
    }
    void connect(int a, int b) {
        id[find(a)] = find(b);
    }
    bool connected(int a, int b) {
        return find(a) == find(b);
    }
};
```

### Merge using Rank

The time complexity of one operation in `UnionFind` with `N` elements, path compression and union by rank is amortized `O(alpha(N))` where `alpha(N)` is the inverse function of Ackermann function. Note that `O(alpha(N))` is even more efficient than `O(logN)`.

```cpp
class UnionFind {
    vector<int> id, rank;
    int cnt; // `cnt` is the number of connected components in the graph
public:
    UnionFind(int n) : id(n), rank(n, 0), cnt(n) {
        iota(begin(id), end(id), 0);
    }
    int find(int a) {
        return id[a] == a ? a : (id[a] = find(id[a]));
    }
    void connect(int i, int j) {
        int p = find(i), q = find(j);
        if (p == q) return;
        if (rank[p] > rank[q]) id[q] = p; // always use the node with GREATER rank as the root
        else {
            id[p] = q;
            if (rank[p] == rank[q]) rank[q]++;
        }
        --cnt;
    }
    bool connected(int i, int j) { return find(i) == find(j); }
    int getCount() { return cnt; }
};
```

### Get Size of Union

We can use a `size` array to keep track of the size of each union.

`cnt` keeps track of the number of unions.

```cpp
class UnionFind {
    vector<int> id, size;
    int cnt;
public:
    UnionFind(int n) : id(n), size(n, 1), cnt(n) {
        iota(begin(id), end(id), 0);
    }
    int find(int a) {
        return id[a] == a ? a : (id[a] = find(id[a]));
    }
    void connect(int a, int b) {
        int x = find(a), y = find(b);
        if (x == y) return;
        id[x] = y;
        size[y] += size[x];
        --cnt;
    }
    int getSize(int a) {
        return size[find(a)];
    }
    int getCount() { return cnt; }
};
```

## Problems

* [1319. Number of Operations to Make Network Connected \(Medium\)](https://leetcode.com/problems/number-of-operations-to-make-network-connected/)
* [990. Satisfiability of Equality Equations \(Medium\)](https://leetcode.com/problems/satisfiability-of-equality-equations/)
* [924. Minimize Malware Spread \(Hard\)](https://leetcode.com/problems/minimize-malware-spread/)
* [947. Most Stones Removed with Same Row or Column \(Medium\)](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/)
* [1202. Smallest String With Swaps (Medium)](https://leetcode.com/problems/smallest-string-with-swaps/)

静态联通性 200 130 952 \(等价类\) 959 990

动态连通性. find和union穿插的. 305

时光倒流 803

懒标记 权值线段树 218 307 308 315 327 493 699 715 732 850 1157 1353

lazy tag / lazy propogation 延迟修改

树状数组 307