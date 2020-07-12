# Floyd Warshall

Floyd-Warshall algorithm is an algorithm for finding the shorted paths in a weighted graph with positive or negative edge weights \(but no negative cycles\). A single execution of the algorithm will find the lengths \(summed weights\) of shortest paths between all pairs of vertices.

Floyd-Warshall algorithm and Bellman-Ford algorithm are both DP algorithms. Floyd-Warshall tries every **vertice** for each pair of vertices to find the shortest path between the vertice pair, while Bellman-Ford tries every **edge** `V-1` times to find the shortest path between a specific source node to all other nodes.

## Algorithm

For each vertice `n`, try to use it to update the min distance between each vertice pair `u, v`.

The time complexity is `O(V^3)`. The space complexity is `O(V^2)`.

The following is the Floyd-Warshall algorithm application on [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance \(Medium\)](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/).

The distances are initialized as `1e6` because there are at most 100 nodes and each edge at most weights `10^4`, so the maximum path weight won't exceed `10^6`.

```cpp
// OJ: https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/
// Author: github.com/lzl124631x
// Time: O(N^3)
// Space: O(N^2)
class Solution {
    vector<vector<int>> floyd(int n, vector<vector<int>> &edges) {
        vector<vector<int>> d(n, vector<int>(n, (int) 1e6));
        for (int i = 0; i < n; ++i) d[i][i] = 0;
        for (auto &e : edges) {
            d[e[0]][e[1]] = d[e[1]][e[0]] = e[2];
        }
        for (int i = 0; i < n; ++i) 
            for (int u = 0; u < n; ++u) 
                for (int v = 0; v < n; ++v) 
                    d[u][v] = min(d[u][v], d[u][i] + d[i][v]);
        return d;
    }
public:
    int findTheCity(int n, vector<vector<int>>& E, int k) {
        auto d = floyd(n, E);
        int ans = 0, minCnt = n;
        for (int i = 0; i < n; ++i) {
            int cnt = 0;
            for (int j = 0; j < n; ++j) {
                if (d[i][j] <= k) ++cnt;
            }
            if (cnt <= minCnt) {
                minCnt = cnt;
                ans = i;
            }
        }
        return ans;
    }
};
```

## Path reconstruction

The Floyd-Warshall algorithm typically only provides the lengths of the paths between all pairs of vertices. We can nodify it to reconstruct the path for between any two endpoint vertices.

```text
let dist be a |V| * |V| array of minimum distances initialized to Infinity
let next be a |V| * |V| array of vertex indices initialized to null

procedure FloydWarshallWithPathReconstruction() is
    for each edge (u, v) do
        dist[u][v] ← w(u, v)  // The weight of the edge (u, v)
        next[u][v] ← v
    for each vertex v do
        dist[v][v] ← 0
        next[v][v] ← v
    for k from 1 to |V| do // standard Floyd-Warshall implementation
        for i from 1 to |V|
            for j from 1 to |V|
                if dist[i][j] > dist[i][k] + dist[k][j] then
                    dist[i][j] ← dist[i][k] + dist[k][j]
                    next[i][j] ← next[i][k]
procedure Path(u, v)
    if next[u][v] = null then
        return []
    path = [u]
    while u ≠ v
        u ← next[u][v]
        path.append(u)
    return path
```

## Problems

* [1462. Course Schedule IV \(Medium\)](https://leetcode.com/problems/course-schedule-iv/)
* [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance \(Medium\)](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)

## Reference

* [https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall\_algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm)

