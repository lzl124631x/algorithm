# Greatest Common Divisor (GCD)

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

## Time Complexity

https://www.geeksforgeeks.org/time-complexity-of-euclidean-algorithm/

It's `O(log(min(A, B)))`

## Problems

* [858. Mirror Reflection (Medium)](https://leetcode.com/problems/mirror-reflection/)
* [2183. Count Array Pairs Divisible by K (Hard)](https://leetcode.com/problems/count-array-pairs-divisible-by-k/)