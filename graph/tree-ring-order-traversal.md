# Tree's Ring order traversal

If a connected graph doesn't have cycle, it's a tree.

Here by ring-order traversal, I mean visiting the leaf nodes first (i.e. 1st ring nodes), then the non-leaf nodes next to 1st ring nodes (i.e. 2nd ring nodes), and so on.

## Algorithm

We keep track of indegrees of nodes. Remove those with indegrees 1 from the graph and update the indegrees of the remaining nodes. We keep doing this until exhausting all the nodes.

## Implementation

The following solution is for [310. Minimum Height Trees (Medium)](https://leetcode.com/problems/minimum-height-trees/) which is asking for the nodes 

```cpp
// OJ: https://leetcode.com/problems/minimum-height-trees
// Author: github.com/lzl124631x
// Time: O(V + E)
// Space: O(V + E)
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& E) {
        if (n == 1) return { 0 };
        vector<int> indegree(n), ans;
        vector<vector<int>> G(n);
        for (auto &e : E) {
            int u = e[0], v = e[1];
            indegree[u]++;
            indegree[v]++;
            G[u].push_back(v);
            G[v].push_back(u);
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (indegree[i] == 1) q.push(i);
        }
        while (n > 2) {
            int cnt = q.size();
            while (cnt--) {
                int u = q.front();
                q.pop();
                --n;
                for (int v : G[u]) {
                    if (--indegree[v] == 1) q.push(v);
                }
            }
        }
        while (q.size()) {
            ans.push_back(q.front());
            q.pop();
        }
        return ans;
    }
};
```

## Problems

*  [310. Minimum Height Trees (Medium)](https://leetcode.com/problems/minimum-height-trees/)