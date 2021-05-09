# Topological Sort

Topological sorting for Directed Acyclic Graph \(DAG\) is a linear ordering of vertices such that for every directed edge `uv`, vertex `u` comes before `v` in the ordering.

## Implementation

We represent the graph `G` as `unordered_map<int, vector<int>>` which is a map from source node to a list of destination nodes.

If `u` must happens before `v`, or in other words, `v` is dependent on `u`, then there is a directed edge `u -> v`, where `u` is the source node, and `v` is the destination node.

### Kahn Algorithm \(BFS\)

It requires additional space for storing the `indegree`s of the nodes.

1. Put all the vertices with 0 in-degree in to a `queue q`.
2. Get a vertex `u` at a time from `q`, and decrement the in-degree of all its neighbors.
3. If a neighbor has 0 in-degree, add it to `q`.
4. Keep repeating until we exhaust `q`.
5. If the number of visited vertices equals the total number of vertices, it's a DAG; otherwise, there must be a circle in the graph.

```cpp
// OJ: https://leetcode.com/problems/course-schedule-ii/
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
public:
    vector<int> findOrder(int N, vector<vector<int>>& E) {
        unordered_map<int, vector<int>> G;
        vector<int> indegree(N);
        for (auto &e : E) {
            G[e[1]].push_back(e[0]);
            indegree[e[0]]++;
        }
        queue<int> q;
        for (int i = 0; i < N; ++i) {
            if (indegree[i] == 0) q.push(i);
        }
        vector<int> ans;
        while (q.size()) {
            int u = q.front();
            q.pop();
            ans.push_back(u);
            for (int v : G[u]) {
                if (--indegree[v] == 0) q.push(v);
            }
        }
        return ans.size() == N ? ans : vector<int>{};
    }
};
```

### DFS

Each vertex has three states:

1. -1 =  unvisited
2. 0 = being visited in the current DFS session. If we visit a node with state 0, it means there is a circle in the graph.
3. 1 = has been visited in a prevous DFS session and this vertex is not in a circle.

It's a post-order DFS -- the node is pushed into the answer after all its subsequent nodes are visited.

Don't forget to reverse the `ans` before returning.

```cpp
// OJ: https://leetcode.com/problems/course-schedule-ii/
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
    vector<vector<int>> G;
    vector<int> ans, state; // -1 unvisited, 0 visiting, 1 visited
    bool dfs(int u) {
        if (state[u] != -1) return state[u];
        state[u] = 0;
        for (int v : G[u]) {
            if (!dfs(v)) return false;
        }
        ans.push_back(u);
        return state[u] = 1;
    }
public:
    vector<int> findOrder(int n, vector<vector<int>>& E) {
        G.assign(n, {});
        state.assign(n, -1);
        for (auto &e : E) G[e[1]].push_back(e[0]);
        for (int i = 0; i < n; ++i) {
            if (!dfs(i)) return {};
        }
        reverse(begin(ans), end(ans));
        return ans;
    }
};
```

## Problems

* [207. Course Schedule \(Medium\)](https://leetcode.com/problems/course-schedule/)
* [210. Course Schedule II \(Medium\)](https://leetcode.com/problems/course-schedule-ii/)
* [329. Longest Increasing Path in a Matrix \(Hard\)](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)
* [Stable Wall](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ff43/00000000003379bb)
* [Fox And Names](https://codeforces.com/contest/510/problem/C)
* [802. Find Eventual Safe States (Medium)](https://leetcode.com/problems/find-eventual-safe-states/)
* [1857. Largest Color Value in a Directed Graph (Hard)](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/)