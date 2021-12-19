# 0-1 Knapsack (01背包问题)

Given a list of items with weight `w[i]` and value `v[i]`, what's the maximum value you can get given a knapsack with capacity `C`, i.e. it can hold items with at most weight `C` in total. You can pick each item **at most once** \(i.e. either pick an item `i` 0 times or 1 time\).

## Algorithm

Let `dp[i + 1][c]` be the maximum value we can get using the first `i + 1` items \(index from `0` to `i`\).

```text
dp[i + 1][c] = max(
    dp[i][c],             // If we skip `i`-th item
    dp[i][c-w[i]] + v[i]  // If we pick `i`-th item
)

dp[0][c] = 0
```

## Implementation

```cpp
// Time: O(NC)
// Space: O(NC)
vector<vector<int>> dp(N + 1, vector<int>(C + 1));
for (int i = 0; i < N; ++i) {
    for (int c = w[i]; c <= C; ++c) {
        dp[i + 1][c] = max( dp[i][c], dp[i][c - w[i]] + v[i] );
    }
}
return dp[N][C];
```

## Space Optimization

```cpp
// Time: O(NC)
// Space: O(C)
vector<int> dp(C + 1);
for (int i = 0; i < N; ++i) {
    for (int c = C; c >= w[i]; --c) {
        dp[c] = max( dp[c], dp[c - w[i]] + v[i] );
    }
}
return dp[C];
```

## A Constant Optimization

Since we only need `dp[N][C]`, which only requires `dp[N-1][C]` and `dp[N-1][C-w[N-1]]`. So for `i = N - 2` \(the second from the last item\), we only need to loop from `max( w[N-2], C - w[N-1] )` to `C`.

`dp[N-1][C-w[N-1]]` requires `dp[N-2][C]` and `dp[N-2][C-w[N-1]-w[N-2]]`, so for `i = N - 3`, we only need to loop from `max( w[N-3], C - w[N-1] - w[N-2] )` to `C`.

Generalization: for `i`-th item, the inner loop should be from `max( w[i], C - sum(w[j] | i < j < N ) )` to `C`.

```cpp
// Time: O(NC)
// Space: O(C)
vector<int> dp(C + 1);
int sum = accumulate(w.begin(), w.end(), 0);
for (int i = 0; i < N; ++i) {
    sum -= w[i];
    for (int c = C; c >= max(w[i], C - sum); --c) {
        dp[c] = max( dp[c], dp[c - w[i]] + v[i] );
    }
}
return dp[C];
```

## Problems

* [416. Partition Equal Subset Sum \(Medium\)](https://leetcode.com/problems/partition-equal-subset-sum/)
* [474. Ones and Zeroes \(Medium\)](https://leetcode.com/problems/ones-and-zeroes/)
* [494. Target Sum (Medium)](https://leetcode.com/problems/target-sum/)

