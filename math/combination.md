# Combination

$$ C_n^k = \frac{A_n^k}{k!} = \frac{n!}{k! \cdot (n - k)!} = \frac{n\cdot(n-1)\cdot(n-2)\ldots(n-k+1)}{k!} $$

## When `n` is small

We can the following equation:

$$ C_n^k = \frac{n\cdot(n-1)\cdot(n-2)\ldots(n-k+1)}{k!} $$

```cpp
// Author: github.com/lzl124631x
// Time: O(K)
// Space: O(1)
int combination(int k, int n) {
    if (k == 0 || k == n) return 1;
    long ans = 0;
    for (int i = 1, j = n - k + 1; i <= k; ++i, ++j) ans = ans * j / i;
    return ans;
}
```

## When `n` is large -- Dynamic Programming

To avoid overflow, we will be asked to return the answer modulo some prime number (`1e9+7` on LeetCode).

We can't change the above solution to `ans = ans * j / i % mod` because after the previous modulo operation this current `* j / i` operation might cause truncation.

We need to use this equation:

$$ C_n^k = C_{n-1}^{k-1} + C_{n-1}^k $$

This is actually Pascal Triangle.

```
      1
    1   1
   1  2  1
 1  3   3  1
1  4  6  4  1
```

This can be done using DP.

```
dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
dp[i][0] = 1
```
And the answer is `dp[n][k]`.

Since `dp[i][j]` only depends on `dp[i-1][j-1]` and `dp[i-1][j]`, we can use 1D array to store the DP values. The answer is `dp[k]`.

```cpp
// Author: github.com/lzl124631x
// Time: O(NK)
// Space: O(K)
int combination(int k, int n, int mod) {
    if (k > n - k) k = n - k;
    vector<int> dp(k + 1);
    dp[0] = 1;
    for (int i = 1; i <= n; ++i) {
        for (int j = min(i, k); j > 0; --j) dp[j] = (dp[j] + dp[j - 1]) % mod;
    }
    return dp[k];
}
```

## Sum of combinations

We can use the following equation to get some useful conclusions:

$$
(a + b)^n = C_n^0\cdot a^0\cdot b^n + C_n^1\cdot a^1\cdot b^{n-1} + \dots + C_n^n\cdot a^n\cdot b^0
$$

Let `a = 1, b = 1`, we have

$$
C_n^0 + C_n^1 + C_n^2 + \dots + C_n^{n-1} + C_n^n = 2^n
$$

Let `a = 1, b = -1`, we have

$$
C_n^0 - C_n^1 + C_n^2 - C_n^3 + C_n^4 - C_n^5 + \dots = 0
$$

So 

$$
\sum_{p}C_n^p = \sum_{q}C_n^q = 2^{n-1}
$$
where `p` and `q` represent all the even and odd numbers in `[0, n]`, respectively. 

We can use this trick in [1863. Sum of All Subset XOR Totals (Easy)](https://leetcode.com/problems/sum-of-all-subset-xor-totals/)

## Problems

* [1569. Number of Ways to Reorder Array to Get Same BST (Hard)](https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/)