# Bipartite

## Detect if a graph is bipartite

[886. Possible Bipartition (Medium)](https://leetcode.com/problems/possible-bipartition/)

### DFS

```cpp
// OJ: https://leetcode.com/problems/possible-bipartition/
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
    vector<unordered_set<int>> G;
    vector<int> id;
    bool dfs(int u, int prev = 1) {
        if (id[u]) return id[u] != prev;
        id[u] = -prev;
        for (int v : G[u]) {
            if (!dfs(v, id[u])) return false;
        }
        return true;
    }
public:
    bool possibleBipartition(int N, vector<vector<int>>& A) {
        G.assign(N + 1, {});
        id.assign(N + 1, 0);
        for (auto &v : A) {
            G[v[0]].insert(v[1]);
            G[v[1]].insert(v[0]);
        }
        for (int i = 1; i <= N; ++i) {
            if (id[i]) continue;
            if (!dfs(i)) return false;
        }
        return true;
    }
};
```

### BFS

```cpp
// OJ: https://leetcode.com/problems/possible-bipartition/
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
public:
    bool possibleBipartition(int N, vector<vector<int>>& A) {
        vector<vector<int>> G(N + 1);
        vector<int> id(N + 1, 0);
        for (auto &v : A) {
            G[v[0]].push_back(v[1]);
            G[v[1]].push_back(v[0]);
        }
        queue<int> q;
        for (int i = 1; i <= N; ++i) {
            if (id[i]) continue;
            q.push(i);
            id[i] = 1;
            while (q.size()) {
                int u = q.front();
                q.pop();
                for (int v : G[u]) {
                    if (id[v]) {
                        if (id[v] != -id[u]) return false;
                        else continue;
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

* [886. Possible Bipartition (Medium)](https://leetcode.com/problems/possible-bipartition/)