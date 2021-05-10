# Gray Code

Gray code is a binary numeral system where two successive values differ in only one bit.

For example, the sequence of Gray codes for 3-bit numbers is: `000, 001, 011, 010, 110, 111, 101, 100`, so `G(4)=6`.

This code was invented by Frank Gray in 1953.

## Finding Gray Code

Let's look at the bits of number `n` and the bits of number `G(n)`. Notice that `i`-th bit of `G(n)` equals 1 only when `i`-th bit of `n` equals 1 and `i+1`-th bit equals 0 or the other way around (`i`-th bit equals 0 and `i+1`-th bit equals 1). Thus, `G(n) = n XOR (n>>1)`.

For example

```
      G(4) = 6
    G(100) = 110
         n = 0100
      n>>1 = 0010
n ^ (n>>1) = 0110
```


```cpp
int g (int n) {
    return n ^ (n >> 1);
}
```

## Finding reverse Gray Code

Given Gray code `g`, restore the original number `n`.

We will move from the most significant bits to the least significant ones (the least significant bit has index 1 and the most significant bit has index k). The relation between the bits `n[i]` of number `n` and the bits `g[i]` of number `g`:

```
n[k] = g[k]
n[k−1] = g[k−1] XOR n[k] = g[k] XOR g[k-1]
n[k-2] = g[k-2] XOR n[k-1] = g[k] XOR g[k-1] XOR g[k-2]
...
```

The easiest way to write it in code is:

```cpp
int rev_g (int g) {
    int n = 0;
    for (; g; g >>= 1) n ^= g;
    return n;
}
```

## Problems

* [1238. Circular Permutation in Binary Representation (Medium)](https://leetcode.com/problems/circular-permutation-in-binary-representation/)

## References

* https://cp-algorithms.com/algebra/gray-code.html