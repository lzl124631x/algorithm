# Unbounded Knapsack (无限背包问题)

Given a list of items with weight `w[i]` and value `v[i]`, what's the maximum value you can get given a knapsack with capacity `C`, i.e. it can hold items with at most weight `C` in total. You can pick each item **unlimited times**.

## Basic Version

### Algorithm

Let `dp[i + 1][c]` be the maximum value we can get using the first `i + 1` items \(index from `0` to `i`\).

```text
dp[i + 1][c] = max( dp[i][c - k * w[i]] + k * v[i] | k >= 0 && c - k * w[i] >= 0 )
dp[0][c] = 0
```

### Implementation

```cpp
// Time: O(NC^2)
// Space: O(NC)
vector<vector<int>> dp(N + 1, vector<int>(C + 1));
for (int i = 0; i < N; ++i) {
    for (int c = w[i]; c <= C; ++c) {
        for (int k = 0; c - k * w[i] >= 0; ++k) {
            dp[i + 1][c] = max( dp[i + 1][c], dp[i][c - k * w[i]] + k * v[i] );
        }
    }
}
return dp[N][C];
```

## Optimized Version

### Algorithm

We can examine the relationship between `dp[i + 1][c]` and `dp[i + 1][c - w[i]]`.

```text
dp[i + 1][c] = max(
                    dp[i][c],
                    dp[i][c - w[i]] + v[i],
                    dp[i][c - 2 * w[i]] + 2 * v[i],
                    ...
                  )
dp[i + 1][c - w[i]] = max(
                            dp[i][c - w[i]],
                            dp[i][c - 2 * w[i]] + v[i],
                            dp[i][c - 3 * w[i]] + 2 * v[i],
                            ...
                         )
```

So we have

```text
dp[i + 1][c] = max( dp[i][c], dp[i + 1][c - w[i]] + v[i] )
```

### Implementation

```cpp
// Time: O(NC)
// Space: O(NC)
vector<vector<int>> dp(N + 1, vector<int>(C + 1));
for (int i = 0; i < N; ++i) {
    for (int c = w[i]; c <= C; ++c) {
        dp[i + 1][c] = max( dp[i][c], dp[i + 1][c - w[i]] + v[i] );
    }
}
return dp[N][C];
```

### Space Optimization

```cpp
// Time: O(NC)
// Space: O(C)
vector<int> dp(C + 1);
for (int i = 0; i < N; ++i) {
    for (int c = w[i]; c <= C; ++c) {
        dp[c] = max( dp[c], dp[c - w[i]] + v[i] );
    }
}
return dp[C];
```

## Problems

* [518. Coin Change 2 \(Medium\)](https://leetcode.com/problems/coin-change-2/)

