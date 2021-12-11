# Prime Number

An integer is prime if it has exactly two divisors: 1 and itself. Note that 1 is not a prime number.

```cpp
// Author: github.com/lzl124631x
// Time: O(sqrt(N))
// Space: O(1)
bool isPrime(int n) {
    if (n < 2) return false;
    for (int d = 2; d * d <= n; ++d) {
        if (n % d == 0) return false;
    }
    return true;
}
```

## Even-digit palindromes are not primes except 11

All even-digit palindromes, except for 11, are not primes because they are divisible by `11`. For example, `2332 / 11 = 212`

[866. Prime Palindrome (Medium)](https://leetcode.com/problems/prime-palindrome/)