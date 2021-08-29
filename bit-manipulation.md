# Bit Manipulation

## Bitwise NOT

```
~val
```

```text
NOT 0111 (decimal 7)
= 1000 (decimal 8)
```

`~val` is also equivalent to `val != -1`, since `-1` is the only one number whose bitwise NOT is `0`.

So if `val` is some non-negative number, the following:

```cpp
for (int i = val; ~i; --i)
```

is equivalent to

```cpp
for (int i = val; i >= 0; --i)
```

## Set the `i`th bit

```cpp
mask |= 1 << i
```

## Unset the `i`th bit

```cpp
mask &= ~(1 << i)
```

## Toggle the `i`th bit

```cpp
mask ^= 1 << i
```

## Check if `i`th bit is set

```cpp
mask & (1 << i)
// Or
mask >> i & 1 // less typing
```

## Set the `i`th bit with `x`

```cpp
mask = (mask & ~(1 << i)) | (x << i)
```

## Get the lowest bit

```cpp
function lowbit(int x) {
    return x & -x;
}
```

## Count bit 1s

```cpp
__builtin_popcount(n);
```

Note that the `pop` in this function means `population`.

## Count trailing zeros

```cpp
__builtin_ctz(n);
```

## Count leading zeros

```cpp
__builtin_clz(n);
```

## Check if `n` is power of 2

```cpp
__builtin_popcount(n) == 1
// or
(n & (n - 1)) == 0 // & has lower precedence than ==. Must add the parenthesis.
```

## Traverse all the subsets

```cpp
for (int mask = 0; mask < (1 << N); ++mask)
```

## Complement of a subset

```cpp
// Assume `sub` is a subset of `mask`
int complement = mask ^ sub;
// Or
int complement = mask - sub;
```

## Traverse subsets of a set `mask`

```cpp
for (int sub = mask; sub; sub = (sub - 1) & mask) {
    // `sub` is a non-empty subset of `mask`
}
```

Sample output:

```cpp
// mask = 0b1011
1011
1010
1001
1000
0011
0010
0001
```

Or the other direction:

```cpp
for (int other = mask; other; other = (other - 1) & mask) {
    int sub = mask - other;
    // `other` is a non-empty subset of `mask`. `sub` is the complement of `other`.
}
```

Sample output:

```
0000
0001
0010
0011
1000
1001
1010
```

## Given `N` elements, traverse subsets of size `K` (Gosper's Hack)

```cpp
int sub = (1 << k) - 1;
while (sub < (1 << N)) {
    // `sub` has `K` elements and is a subset of `N` elements.
    int c = sub & - sub;
    int r = sub + c;
    sub = (((r ^ sub) >> 2) / c) | r;
}
``` 
 
## Traverse subsets of subsets

Given an array of `N` elements, traverse all its subsets. For each of the subsets, traverse its all non-empty subsets as well.

```cpp
// Time: O(3^N)
for (int mask = 0; mask < (1 << N); ++mask) {
    // `mask` is a subset of `N` elements
    for (int sub = mask; sub; sub = (sub - 1) & mask) {
        // `sub` is a subset of `mask`.
    }
}
```

Note that the time complexity is `O(3^N)` instead of `O(2^N * 2^N) = O(4^N)`.

**Math proof:**

For a subset `mask` with `K` bits of `1`s, there are `2^K` subsets of it.

For `N` elements, there are `C(N, K)` subsets with `K` bits of `1`s.

So the total number is `SUM( C(N, K) * 2^K | 0 <= K <= N )`.

Since `(1 + x)^N = C(N, 0) * x^0 + C(N, 1) * x^1 + ... + C(N, N) * x^N`, let `x = 2`, we have `3^N = SUM( C(N, K) * 2^K | 0 <= K <= N )`.

**Intuitive proof:**

For each bit index `i` (`0 <= i < N`), the `i`-th bits of `mask` and `sub` must be one of `11, 10, 00`, and can't be `01`. So each bit has `3` possibilities, the total complexity is `O(3^N)`.

## Calculate subset sums

```cpp
// Time: O(N * 2^N)
for (int mask = 0; mask < (1 << N); ++mask) {
    for (int i = 0; i < N; ++i) {
        if (mask >> i & 1) sum[mask] += A[i];
    }
}
```

Or 

```cpp
// DP solution
// Time: O(2^N)
for (int mask = 1; mask < (1 << N); ++mask) {
    sum[mask] = sum[mask - lowbit(mask)] + A[__builtin_ctz(lowbit(mask))];
}
```

where `lowbit(x) = x & -x`, and `__builtin_ctz(x)` counts the trailing zeros in `x`. Since `lowbit(mask)` is a power of two, `__builtin_ctz(lowbit(mask)) == log2(lowbit(mask))`.

## Generate `logs` array

`logs[n]` is `floor(log2(n))`.

```
logs[1] = 0
logs[2] = 1
logs[3] = 1
logs[4] = 2
logs[5] = 2
logs[6] = 2
logs[7] = 2
logs[8] = 3
...
```

```cpp
// Time: O(2^N)
for (int mask = 2; mask < (1 << N); ++mask) {
    logs[mask] = logs[mask >> 1] + 1;
}
```

## Reference

* [https://oi-wiki.org/math/bit/](https://oi-wiki.org/math/bit/)

