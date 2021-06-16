# Greedy Scheduling

```cpp
// Sort by start time
priority_queue<int> pq; // Customized priority
int time = 0;
for (int i = 0; i < N && pq.size(); ) {
    if (pq.empty()) time = max(time, A[i][0]); // if no available task, take the starting time of the next task
    while (i < N && A[i][0] <= time) pq.push(A[i++][1]); // keep taking in tasks start before/at the current time.
    // access the pq.top()
    pq.pop();
    // update time
    while (pq.size() && pq.top() < time) pq.pop(); // evict the tasks that ends before the current time
}
```

* [1834. Single-Threaded CPU (Medium)](https://leetcode.com/problems/single-threaded-cpu/)
* [1353. Maximum Number of Events That Can Be Attended (Medium)](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)