# Gcd

## Euclidean algorithm

```text
gcd(a, 0) = a
gcd(a, b) = gcd(b, a % b)
```

Recursive:

```cpp
int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}
```

Iterative:

```cpp
int gcd(int a, int b) {
    if (b) while ((a %= b) && (b %= a));
    return a + b;
}
```

## C++ Built-in function

C++ has a built-in `__gcd` function.

```cpp
__gcd(6, 20); // 2
```

Since C++11, we can directly use `gcd`.

## Problems

* [858. Mirror Reflection (Medium)](https://leetcode.com/problems/mirror-reflection/)