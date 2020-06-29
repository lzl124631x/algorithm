# Fast Mod

## Implementation

```cpp
int modpow(int base, int exp, int mod) {
    base %= mod;
    long ans = 1;
    while (exp > 0) {
        if (exp & 1) ans = (ans * base) % mod;
        base = ((long)base * base) % mod;
        exp >>= 1;
    }
    return ans;
}
```

Or simply pre-compute those pows.

```cpp
vector<int> p(N, 1);
for (int i = 1; i < N; ++i) p[i] = (p[i - 1] * 2) % mod;
```