# Eulerian Path

An Eulerian path (欧拉路径; 一笔画问题) is a path visiting every **edge** exactly once.

Any connected directed graph where all nodes have equal in-degree and out-degree has an Eulerian circuit \(an Eulerian path ending where it started.\)

If the end point is the same as the starting point, this Eulerian Path is called an **Eulerian Circuit** (欧拉回路).

## Detecting Eulerian Path

Undirected Graph:
* If there are only TWO nodes with odd degrees and all other nodes have even degrees, then there must exist an Eulerian Path (NOT Circuit) from one of the odd-degree node to the other odd-degree node.
* If all nodes have even degrees, this Eulerian Path is also a Eulerian Circuit.

Directioned Graph:
* If there is at most one node `u` whose `outdegree[u] = indegree[u] + 1`, and at most one node `v` whose `indegree[v] = outdegree[v] + 1`, then there must exists an Eulerian Path from `u` (If `u` doesn't exist, any node works) to `v` (If `v` doesn't exist, any node works).
* If every nodes `n` has `indegree[n] = outdegree[n]`, then there is an Eulerian Circuit.

## Finding Eulerian Circuit: Hierholzer’s Algorithm

[Hierholzer’s Algorithm for directed graph](https://www.geeksforgeeks.org/hierholzers-algorithm-directed-graph/)

We can find the circuit/path in `O(E)`, i.e. linear time.

The algorithem is as follows \([wiki](https://en.wikipedia.org/wiki/Eulerian_path#Hierholzer.27s_algorithm)\):

* Choose any starting vertex `v`, and follow a trail of edges from that vertex until returning to `v`. It is not possible to get stuck at any vertex other than `v`, because the even degree of all vertices ensures that, when the trail enters another vertex `w` there must be an unused edge leaving `w`. The tour formed in this way is a closed tour, but may not cover all the vertices and edges of the initial graph.
* As long as there exists a vertex `u` that belongs to the current tour but that has adjacent edges not part of the tour, start another trail from `u`, following unused edges until returning to `u`, and join the tour formed in this way to the previous tour.
* Since we assume the original graph is connected, repeating the previous step will exhaust all edges of the graph.


This is a Post-order DFS.

The following code is for [2097. Valid Arrangement of Pairs (Hard)](https://leetcode.com/problems/valid-arrangement-of-pairs/)

```cpp
// OJ: https://leetcode.com/problems/valid-arrangement-of-pairs/
// Author: github.com/lzl124631x
// Time: O(N + U) where N is the number of pairs and U is the number of unique numbers in the pairs
// Space: O(N + U)
class Solution {
public:
    vector<vector<int>> validArrangement(vector<vector<int>>& E) {
        int N = E.size();
        unordered_map<int, vector<int>> G;
        unordered_map<int, int> indegree, outdegree;
        G.reserve(N);
        indegree.reserve(N);
        outdegree.reserve(N);
        for (auto &e : E) {
            int u = e[0], v = e[1];
            outdegree[u]++;
            indegree[v]++;
            G[u].push_back(v);
        }
        int start = -1;
        for (auto &[u, vs] : G) {
            if (outdegree[u] - indegree[u] == 1) {
                start = u; // If there exists one node `u` that `outdegree[u] = indegree[u] + 1`, use `u` as the start node.
                break;
            }
        }
        if (start == -1) start = E[0][0]; // If there doesn't exist such node `u`, use any node as the start node
        vector<vector<int>> ans;
        function<void(int)> euler = [&](int u) {
            auto &vs = G[u];
            while (vs.size()) {
                int v = vs.back();
                vs.pop_back();
                euler(v);
                ans.push_back({ u, v }); // Post-order DFS. The edge is added after node `v` is exhausted
            }
        };
        euler(start);
        reverse(begin(ans), end(ans)); // Need to reverse the answer array in the end.
        return ans;
    }
};
```

## Problems

* [332. Reconstruct Itinerary (Hard)](https://leetcode.com/problems/reconstruct-itinerary/)
* [753. Cracking the Safe \(Hard\)](https://leetcode.com/problems/cracking-the-safe/)
* [2097. Valid Arrangement of Pairs (Hard)](https://leetcode.com/problems/valid-arrangement-of-pairs/)
