# Bipartite

A bipartite graph (or bigraph) is a graph whose vertices can be divided into two disjoint and independent sets U and V such that every edge connects a vertex in U to one in V. Vertex sets U and V are usually called the parts of the graph. Equivalently, a bipartite graph is a graph that does not contain any odd-length cycles.

![Bipartite](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e8/Simple-bipartite-graph.svg/220px-Simple-bipartite-graph.svg.png)

## Is graph bipartite?

[785. Is Graph Bipartite? \(Medium\)](https://leetcode.com/problems/is-graph-bipartite/)

### DFS

**DFS to jump between two parts and assign part number to nodes**

Take each node in the Graph as starting point of DFS.

Assign the nodes to one of two parts, `-1` and `1`, in an alternating order.

If a node is already assigned to a part, once it's visited again, its part should be the same as the part of the previous node.

```cpp
// OJ: https://leetcode.com/problems/is-graph-bipartite/
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
public:
    bool isBipartite(vector<vector<int>>& G) {
        vector<int> id(G.size());
        function<bool(int, int)> dfs = [&](int u, int prev) {
            if (id[u]) return id[u] != prev; // This node's part should be the same as the part of the previous part
            id[u] = -prev; // Assign nodes to one of the two parts, -1 or 1
            for (int v : G[u]) {
                if (!dfs(v, id[u])) return false;
            }
            return true;
        };
        for (int i = 0; i < G.size(); ++i) {
            if (id[i]) continue; // This node is already assigned to a part
            if (!dfs(i, 1)) return false;
        }
        return true;
    }
};
```

### BFS

Take each node in the Graph as starting point of BFS.

Assign nodes to part `-1` or `1` layer by layer in an alternating order.

If a node is already assigned to a part, once it's visited again, its part should be the same as the part of the previous node.

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
            if (id[i]) continue; // This node is already assigned to a part
            q.push(i);
            id[i] = 1; // Assign this starting node to part 1
            while (q.size()) {
                int u = q.front();
                q.pop();
                for (int v : G[u]) {
                    if (id[v]) {
                        if (id[v] != -id[u]) return false; // Two neighboring nodes shouldn't be in the same part
                        continue;
                    }
                    id[v] = -id[u]; // Assign neighbor node to the other part
                    q.push(v);
                }
            }
        }
        return true;
    }
};
```

## Problems

* [785. Is Graph Bipartite? \(Medium\)](https://leetcode.com/problems/is-graph-bipartite/)
* [886. Possible Bipartition \(Medium\)](https://leetcode.com/problems/possible-bipartition/)

