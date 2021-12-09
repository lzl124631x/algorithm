# Prime Number

An integer is prime if it has exactly two divisors: 1 and itself. Note that 1 is not a prime number.

```cpp
// Author: github.com/lzl124631x
// Time: O(sqrt(N))
// Space: O(1)
bool isPrime(int n) {
    if (n == 1) return false;
    for (int d = 2; d * d <= n; ++d) {
        if (n % d == 0) return false;
    }
    return true;
}
```