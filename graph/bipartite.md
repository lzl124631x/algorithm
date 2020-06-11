# Bipartite

## Is graph bipartite?

[785. Is Graph Bipartite? (Medium)](https://leetcode.com/problems/is-graph-bipartite/)

### DFS

```cpp
// OJ: https://leetcode.com/problems/is-graph-bipartite/
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
    bool dfs(vector<vector<int>> &G, vector<int> &id, int u, int prev = 1) {
        if (id[u]) return id[u] != prev;
        id[u] = -prev;
        for (int v : G[u]) {
            if (!dfs(G, id, v, id[u])) return false;
        }
        return true;
    }
public:
    bool isBipartite(vector<vector<int>>& G) {
        vector<int> id(G.size());
        for (int i = 0; i < G.size(); ++i) {
            if (id[i]) continue;
            if (!dfs(G, id, i)) return false;
        }
        return true;
    }
};
```

### BFS

```cpp
// OJ: https://leetcode.com/problems/is-graph-bipartite/
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
public:
    bool isBipartite(vector<vector<int>>& G) {
        vector<int> id(G.size());
        queue<int> q;
        for (int i = 0; i < G.size(); ++i) {
            if (id[i]) continue;
            q.push(i);
            id[i] = 1;
            while (q.size()) {
                int u = q.front();
                q.pop();
                for (int v : G[u]) {
                    if (id[v]) {
                        if (id[v] != -id[u]) return false;
                        continue;
                    }
                    id[v] = -id[u];
                    q.push(v);
                }
            }
        }
        return true;
    }
};
```

## Problems

* [785. Is Graph Bipartite? (Medium)](https://leetcode.com/problems/is-graph-bipartite/)
* [886. Possible Bipartition (Medium)](https://leetcode.com/problems/possible-bipartition/)