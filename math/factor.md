# Factors

Get all the prime factors of `n`.

## Implementation

```cpp
vector<int> factors(int n) {
    vector<int> ans;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i) continue;
        ans.push_back(i);
        while (n % i == 0) n /= i;
    }
    if (n > 1 || ans.empty()) ans.push_back(n);
    return ans;
}
```

## Problems

* [952. Largest Component Size by Common Factor (Hard)](https://leetcode.com/problems/largest-component-size-by-common-factor/)