# Factorial

$$ n! = 1 \times 2 \times 3 \times \dots \times (n - 1) \times n $$

```cpp
typedef long long int64;
const int N = 1e5 + 10;
int fact[N], ifact[N], inv[N];
struct comb_init {
    comb_init() {
        inv[1] = 1;
        for (int i = 2; i < N; ++i) {
            inv[i] = (MOD - MOD / i) * (int64)inv[MOD % i] % MOD;
        }
        fact[0] = ifact[0] = 1;
        for (int i = 1; i < N; ++i) {
            fact[i] = (int64)fact[i - 1] * i % MOD;
            ifact[i] = (int64)ifact[i - 1] * inv[i] % MOD;
        }
    }
} comb_init_;

int64 comb(int n, int m) {
    if (n < m || m < 0) return 0;
    return (int64)fact[n] * ifact[m] % MOD * ifact[n - m] % MOD;
}
```

## Problems

* [1735. Count Ways to Make Array With Product (Hard)](https://leetcode.com/problems/count-ways-to-make-array-with-product/)