# Floyd Warshall Algorithm

Floyd-Warshall algorithm is an algorithm for finding the shorted paths in a weighted graph with positive or negative edge weights (but no negative cycles). A single execution of the algorithm will find the lengths (summed weights) of shortest paths between all pairs of vertices.

## Algorithm

For each vertice `n`, try to use it to update the min distance between each vertice pair `u, v`.

The time complexity is `O(V^3)`. The space complexity is `O(V^2)`.

The following is the Floyd-Warshall algorithm application on [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance (Medium)](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/).

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

## Problems

* [1462. Course Schedule IV (Medium)](https://leetcode.com/problems/course-schedule-iv/)
* [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance (Medium)](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)