# Palindrome

## Common solutions

When we need to check whether a substring `s[i..j]` is a palindrome for all pairs of `i, j`, we can use the following solutions to reduce the time complexity from the brute force `O(N^3)` to `O(N^2)`.

###  Rolling Hash

Calculate the hash in both directions and then compare the hash.

Example: [1745. Palindrome Partitioning IV (Hard)](https://github.com/lzl124631x/LeetCode/tree/master/leetcode/1745.%20Palindrome%20Partitioning%20IV)

### DP table

Let `dp[i][j]` be whether substring `s[i..j]` is a palindrome.

```
dp[i][j] = s[i] == s[j] && dp[i + 1][j - 1]
dp[i][i] = true 
```

Example: [1745. Palindrome Partitioning IV (Hard)](https://github.com/lzl124631x/LeetCode/tree/master/leetcode/1745.%20Palindrome%20Partitioning%20IV)