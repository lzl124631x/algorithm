# 0-1 Knapsack problem

Given a list of items with weight `w[i]` and value `v[i]`, what's the maximum value you can get given a knapsack with capacity `C`, i.e. it can hold items with at most weight `C` in total. You can pick each item **at most once** (i.e. either pick an item `i` 0 times or 1 time).

Let `dp[i][w]` be the maximum value we can get using the first `i` items (index from `0` to `i - 1`).

```
dp[i][c] = max(
    dp[i-1][c],             // If we skip `i`-th item
    dp[i-1][c-w[i]] + v[i]  // If we pick `i`-th item
)

dp[0][c] = 0
```

## Problems

* [416. Partition Equal Subset Sum (Medium)](https://leetcode.com/problems/partition-equal-subset-sum/)
* [474. Ones and Zeroes (Medium)](https://leetcode.com/problems/ones-and-zeroes/)