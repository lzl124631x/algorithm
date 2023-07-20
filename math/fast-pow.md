# Fast Pow

Take `2^5` for example, $2^5 = 2^{4+1} = 2^4 * 2^1$

Initially, `base = 2, ans = 1, exp = 5 = (101)`

Here, the rightmost `1` means that we should multiply `ans` by `2^1`.

The middle `0` means that we should NOT multiply `ans` by `2^2`.

The leftmost `1` means that we should multiply `ans` by `2^4`.

In sum, we can keep doing `exp >>= 1` and `base = (base * base)`. We only do `ans = (ans * base)` when `exp & 1 == 1`.

## Implementation

```cpp
// Time: O(logE)
// Space: O(1)
int modpow(int base, int exp, int mod) {
    base %= mod;
    long ans = 1;
    while (exp > 0) {
        if (exp & 1) ans = ans * base % mod;
        base = (long)base * base % mod;
        exp >>= 1;
    }
    return ans;
}
```

Or simply pre-compute those pows.

```cpp
vector<int> p(exp + 1, 1);
for (int i = 1; i <= exp; ++i) p[i] = ((long long) p[i - 1] * base) % mod;
```

If we want to reuse the result across test cases, we can use `static vector`.

```cpp
static vector<int> p{1};
while (p.size() <= exp) p.push_back(((long long) p.back() * base) % mod);
```

## Problems

* [1498. Number of Subsequences That Satisfy the Given Sum Condition \(Medium\)](https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/)

