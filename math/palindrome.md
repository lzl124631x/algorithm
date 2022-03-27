# Palindrome

An integer is a palindrome if it reads the same from left to right as it does from right to left.

## Check whether a number is a Palindrome

```cpp
// Author: github.com/lzl124631x
// Time: O(lgN)
// Space: O(1)
bool isPalindrome(long n) {
    long r = 0;
    for (long tmp = n; tmp; tmp /= 10) r = r * 10 + tmp % 10;
    return r == n;
}
```

## Generate Increasing Palindrome Numbers

[866. Prime Palindrome (Medium)](https://leetcode.com/problems/prime-palindrome/) is a good problem for testing the code for generating palindromes.

```cpp
// OJ: https://leetcode.com/problems/prime-palindrome/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(1)
class Solution {
    int getPalindrome(int half, bool odd) {
        int pal = half;
        if (odd) half /= 10;
        for (; half; half /= 10) pal = pal * 10 + half % 10;
        return pal;
    }
    bool isPrime(int n) {
        if (n < 2) return false;
        for (int d = 2; d * d <= n; ++d) {
            if (n % d == 0) return false;
        }
        return true;
    }
public:
    int primePalindrome(int n) {
        for (int len = 1; true; ++len) {
            int begin = pow(10, (len - 1) / 2), end = pow(10, (len + 1) / 2);
            for (int half = begin; half < end; ++half) {
                int pal = getPalindrome(half, len % 2);
                if (pal >= n && isPrime(pal)) return pal;
            }
        }
        return -1;
    }
};
```

## Problems

* [479. Largest Palindrome Product (Hard)](https://leetcode.com/problems/largest-palindrome-product/)
* [866. Prime Palindrome (Medium)](https://leetcode.com/problems/prime-palindrome/)
* [906. Super Palindromes (Hard)](https://leetcode.com/problems/super-palindromes/)
* [2081. Sum of k-Mirror Numbers (Hard)](https://leetcode.com/problems/sum-of-k-mirror-numbers/)
* [2217. Find Palindrome With Fixed Length (Medium)](https://leetcode.com/problems/find-palindrome-with-fixed-length/)