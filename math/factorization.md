# Factorization

Get all the prime factors of `n`.

## Implementation

```cpp
vector<int> factors(int n) {
    vector<int> ans;
    for (int d = 2; d * d <= n; ++d) { // This `d * d <= n` is key to reduce time complexity.
        if (n % d == 0) ans.push_back(d); // if `n` is divisible by `d`, add `d` as a factor
        while (n % d == 0) n /= d; // keep removing this factor from `n` until `n` is no longer divisible by `d`
    }
    if (n > 1) ans.push_back(n); // If `n` is still not `1`, then the remaining `n` is a prime factor.
    return ans;
}
```

## Problems

* [952. Largest Component Size by Common Factor (Hard)](https://leetcode.com/problems/largest-component-size-by-common-factor/)

## Reference

https://cp-algorithms.com/algebra/factorization.html