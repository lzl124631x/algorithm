# Dijkstra

Dijkstra's original algorithm found the shortest path between two given nodes, but a more common variant fixes a single node as the "source" node and finds shortest paths from the source to all other nodes in the graph, producing a **shortest-path tree**.

It's a **greedy** BFS algorithm.

It does **NOT** support negative edge.

Note that we should only use this algorithm on a **weighted** graph. If it's an unweighted graph, just use BFS.

## Algorithm

**Summary: Use heap to store the cost of nodes. Each time pop the heap top node, whose shortest path is determimed once popped, and use its outbound edges to relax its neighbors and push the relaxed neighbors into the heap.**

Given `V` vertices and `E` edges. The all weights of all edges are non-negative. Find the smallest distance from a source node `S` to all other nodes.

We split the `V` vertices into two sets:
* `A`, visited nodes, i.e. those whose shortest path is already determined.
* `B`, unvisited nodes.

Initially:
* `A` is empty and `B` contains all the vertices
* The distance of the source vertex is `0`, and `Infinity` for all other vertices.

Then we keep the following operations until all vertices are visited:
1. Find the vertice with the smallest distance from `B`, say `u`. Move `u` from `B` to `A`.
2. For each outbound edge from `u` `(u, v, w)` (`v` is the destination verte, `w` is the weight), try to relax `v` using `u` and `w` -- If `dist[v] > dist[u] + w`, then `dist[v] = dist[u] + w`.

## Why no negative edge?

Dijkstra is a greedy algorithm. If all the edges are non-negative, once we find the node with the smallest cost from unvisited vertice set, say `u`, we are sure that it won't be updated later -- it's already the optimal path.

But if there are negative edges, this `u` can be updated again later using negative edge.

## Implementation

The most basic implementation of Dijkstra algorithm has time complexity `O(V^2)`. Here I only talk about the solution using `priority_queue`.

Let `pq` be a `priority_queue` of pairs of the cost \(i.e. the length of the shortest path from `src`\) and the vertex index. The element with the smallest cost is at the top of the queue \(i.e. min-heap\). Initially `pq` only has `(0, src)` in it.

Let `dist[u]` be the cost of `u`. Initially `dist[src] = 0` and `dist[u] = INF` for all other vertices.

```cpp
// Time: O(ElogE)
// Space: O(E)
typedef unordered_map<int, unordered_map<int, int>> Graph; // map from source node to <destination node, edge cost>
typedef pair<int, int> iPair;
vector<int> dijkstra(Graph &graph, int N, int source) {
    priority_queue<iPair, vector<iPair>, greater<iPair>> pq; // cost, nodeIndex. Min-heap -- the unvisited ndoe with the smallest distance is at the top
    vector<int> dists(N, INT_MAX), seen(N);
    dists[source] = 0;
    pq.emplace(0, source); // push the source node into heap
    while (pq.size()) {
        int u = pq.top().second; // greedily visit the unvisited node with smallest cost, and relax its neighbors
        pq.pop();
        if (seen[u]) continue; // `u` has been visited, skip
        seen[u] = 1; // mark `u` as visited
        for (auto &neighbor : graph[u]) { // try to use node `u` to relax its neighbors
            int v = neighbor.first, weight = neighbor.second;
            if (!seen[u] && dists[v] > dists[u] + weight) { // If `v` is not visited and can be relaxed
                dists[v] = dists[u] + weight; // update the cost
                pq.emplace(dists[v], v); // push the updated pair of <cost, index> into the heap
            }
        }
    }
    return dists;
}
```

When the same node has been relaxed multiple times, we should delete the old relaxations from the priority queue. But with `priority_queue`, we don't have a way to do it. So we just push the entry of the same node and smaller cost into the priority queue.

When a node is popped from the queue and it's already visited, then we should skip it.

Time complexity analysis: In the worst case, each visited node will push new items into the priority queue using all its outbound edges. So there will be `O(E)` items in the priority queue in total. The overall time complexity is `Elog(E)`.

If we can use Fabonacci Heap, the time complexity will be reduced to `O(E + VlogV)`.

Another way to prevent visiting the same node twice is as follows:

```cpp
// Time: O(ElogE)
// Space: O(E)
typedef unordered_map<int, unordered_map<int, int>> Graph;
typedef pair<int, int> iPair;
vector<int> dijkstra(Graph &graph, int N, int source) {
    priority_queue<iPair, vector<iPair>, greater<iPair>> pq;
    vector<int> dists(N, INT_MAX);
    dists[source] = 0;
    pq.emplace(0, source);
    while (pq.size()) {
        int u = pq.top().second, cost = pq.top().first;
        pq.pop();
        if (cost > dist[u]) continue; // this is already not the optimal, skip
        for (auto &neighbor : graph[u]) {
            int v = neighbor.first, weight = neighbor.second;
            if (dists[v] > dists[u] + weight) {
                dists[v] = dists[u] + weight;
                pq.emplace(dists[v], v);
            }
        }
    }
    return dists;
}
```

## Problems

* [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/)
* [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
* [1514. Path with Maximum Probability (Medium)](https://leetcode.com/problems/path-with-maximum-probability/)
* [1368. Minimum Cost to Make at Least One Valid Path in a Grid (Hard)](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

## Reference

* [Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

