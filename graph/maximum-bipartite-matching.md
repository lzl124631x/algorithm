# Maximum Bipartite Matching

## Hungarian Maximum Matching Algorithm

```cpp
// OJ: https://leetcode.com/problems/maximum-number-of-accepted-invitations/
// Author: github.com/lzl124631x
// Time: O(?)
// Space: O(N)
class Solution {
public:
    int maximumInvitations(vector<vector<int>>& A) {
        int M = A.size(), N = A[0].size(), ans = 0;
        vector<int> match(N, -1);
        vector<bool> seen;
        function<bool(int)> dfs = [&](int u) {
            for (int v = 0; v < N; ++v) {
                if (!A[u][v] || seen[v]) continue; // If there is no edge between (u, v), or this girl is visited already, skip
                seen[v] = true;
                if (match[v] == -1 || dfs(match[v])) {
                    match[v] = u;
                    return true;
                }
            }
            return false;
        };
        for (int i = 0; i < M; ++i) { // Try each node as the starting point of DFS
            seen.assign(N, false);
            if (dfs(i)) ++ans;
        }
        return ans;
    }
};
```

## Problems

* [1820. Maximum Number of Accepted Invitations (Medium)](https://leetcode.com/problems/maximum-number-of-accepted-invitations/)

## Reference

* [Kuhn's Algorithm for Maximum Bipartite Matching](https://cp-algorithms.com/graph/kuhn_maximum_bipartite_matching.html)
* [二分图最大匹配](https://oi-wiki.org/graph/graph-matching/bigraph-match/)
* [Hungarian Maximum Matching Algorithm](https://brilliant.org/wiki/hungarian-matching/)