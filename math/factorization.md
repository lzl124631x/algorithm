# Factorization

Get all the prime factors of `n`.

## Implementation

```cpp
vector<int> factors(int n) {
    vector<int> ans;
    for (int d = 2; d * d <= n; ++d) {
        if (n % d) continue; // if `n` is not divisible by `d`, skip
        ans.push_back(d); // this is a factor of `n`.
        while (n % d == 0) n /= d; // remove this factor from `n`.
    }
    if (n > 1) ans.push_back(n); // If `n` is still not `1`, then the remaining `n` is a prime factor.
    return ans;
}
```

## Problems

* [952. Largest Component Size by Common Factor (Hard)](https://leetcode.com/problems/largest-component-size-by-common-factor/)

## Reference

https://cp-algorithms.com/algebra/factorization.html