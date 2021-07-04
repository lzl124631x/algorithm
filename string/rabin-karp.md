# Rabin Karp

A string-searching algorithm that uses rolling hash to find an exact match of a pattern string in a text.

The expected time complexity is `O(S+T)` where `S` and `T` are the lengths of the original string `s` and the pattern string `t` respectively.

Its worst-case time complexity is `O(ST)`.

## Algorithm

First get the hash `ht` of the pattern string `t`.

We keep a sliding window of size `T` and compute the hash `hs` of the substring of `s` within the window.

Whenever `ht == hs`, the part in the window might be the same as `t`. In case of hash conflict, we linearly check the equity of them.

## Implementation

### Version 1

* The hashes are `signed long long`.
* We need to use a number `m` to mod the hashes in case of overflow.
* We need to do `hs += m` when `hs < 0`.

```cpp
// OJ: https://leetcode.com/problems/implement-strstr/
// Author: github.com/lzl124631x
// Time: average O(S+T), worst O(ST)
// Space: O(1)
class Solution {
    typedef long long LL;

public:
    int strStr(string s, string t) {
        int S = s.size(), T = t.size(), d = 128; 
        if (!S || !T || T > S) return T ? -1 : 0;
        LL m = 1e9+7, p = 1, hs = s[0], ht = t[0];
        for (int i = 1; i < T; ++i) {
            p = (p * d) % m; // we need d^(T-1)
            ht = (ht * d + t[i]) % m;
            hs = (hs * d + s[i]) % m;
        }
        for (int i = 0; i <= S - T; ++i) { // Loop for each start/pop point
            if (hs == ht) { // in case of collision, check the equity.
                int j = 0;
                for (; j < T && s[i + j] == t[j]; ++j);
                if (j == T) return i;
            }
            if (i < S - T) hs = ((hs - s[i] * p) * d + s[i + T]) % m;
            if (hs < 0) hs += m; // we might get negative hs, converting it to positive
        }
        return -1;
    }
};
```

## Version 2

* This version is translated from standard golang implementation.
* The hashes are `unsigned int`.
* No mod number `m` is needed! It will automatically mod by `2^32` when overflow happens.

```cpp
// OJ: https://leetcode.com/problems/implement-strstr/
// Author: github.com/lzl124631x
// Time: average O(S+T), worst O(ST)
// Space: O(T)
// Ref: https://github.com/golang/go/blob/ba9e10889976025ee1d027db6b1cad383ec56de8/src/internal/bytealg/bytealg.go#L128
class Solution {
    const unsigned primeRK = 16777619;
    pair<unsigned, unsigned> hashStr(string &s) {
        unsigned h = 0, p = 1, sq = primeRK;
        for (char c : s) h = h * primeRK + c;
        for (int i = s.size(); i > 0; i >>= 1) { // fast pow
            if (i & 1) p *= sq;
            sq *= sq;
        }
        return { h, p };
    }
public:
    int strStr(string s, string t) {
        if (t.empty()) return 0;
        unsigned T = t.size(), hs = 0;
        auto [ht, p] = hashStr(t);
        for (int i = 0; i < T; ++i) hs = hs * primeRK + s[i];
        if (hs == ht && s.substr(0, T) == t) return 0;
        for (int i = T; i < s.size(); ) {
            hs = hs * primeRK + s[i] - p * s[i - T];
            ++i;
            if (hs == ht && s.substr(i - T, T) == t) return i - T;
        }
        return -1;
    }
};
```

### Version 3

* The hashes are `unsigned int`.
* No mod number `m` is needed! It will automatically mod by `2^32` when overflow happens.

Note that we should use `unsigned long long` for the hash to avoid hash conflict. For example in [1923. Longest Common Subpath (Hard)](https://leetcode.com/problems/longest-common-subpath/).

```cpp
// OJ: https://leetcode.com/problems/implement-strstr/
// Author: github.com/lzl124631x
// Time: average O(S + T), worst O(ST)
// Space: O(1)
class Solution {
public:
    int strStr(string s, string t) {
        unsigned long long S = s.size(), T = t.size(), d = 16777619, p = 1, hs = 0, ht = 0; // we can use d = 29 as well or some other prime greater than the size of the character set.
        if (!S || !T || T > S) return T ? -1 : 0;
        for (int i = 0; i < S; ++i) {
            hs = hs * d + s[i] - 'a';
            if (i < T) {
                p *= d;
                ht = ht * d + t[i] - 'a';
            }
            if (i >= T) hs -= (s[i - T] - 'a') * p;
            if (i >= T - 1 && hs == ht) {
                int j = 0, start = i - T + 1;
                for (; j < T && s[start + j] == t[j]; ++j);
                if (j == T) return start;
            }
        }
        return -1;
    }
};
```

### Why 16777619?

`16777619` is the FNV(Fowler–Noll–Vo) prime for 32 bit integer. When using FNV prime, the hash values produced are more scattered throughout the hash space.

See more in [FNV prime](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function#FNV_prime)

## Application

Rabin Karp algorithm or rolling hash is often used in problems where you need to find repeated subarray/substring in a long array/string.

Examples:

* [718. Maximum Length of Repeated Subarray \(Medium\)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)
* [1044. Longest Duplicate Substring (Hard)](https://leetcode.com/problems/longest-duplicate-substring/)

The following function is used in [718. Maximum Length of Repeated Subarray \(Medium\)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/) to generate the rolling hash.

```cpp
vector<int> rolling(vector<int> &A, int len) {
    vector<int> ans(A.size() - len + 1);
    long h = 0, p = 1, mod = 1e9+7, d = 113;
    for (int i = 0; i < A.size(); ++i) {
        h = (h * d + A[i]) % mod;
        if (i < len - 1) p = (p * d) % mod;
        else {
            ans[i - len + 1] = h;
            h = (h - A[i - len + 1] * p) % mod;
            if (h < 0) h += mod;
        }
    }
    return ans;
}
```

## Problem

* [28. Implement strStr\(\) \(Easy\)](https://leetcode.com/problems/implement-strstr/)
* [1044. Longest Duplicate Substring \(Hard\)](https://leetcode.com/problems/longest-duplicate-substring/)
* [718. Maximum Length of Repeated Subarray \(Medium\)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)
* [214. Shortest Palindrome (Hard)](https://leetcode.com/problems/shortest-palindrome/)
* [1297. Maximum Number of Occurrences of a Substring (Medium)](https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring/)
* [1923. Longest Common Subpath (Hard)](https://leetcode.com/problems/longest-common-subpath/)

## Reference

* [https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm)
* [https://www.youtube.com/watch?v=H4VrKHVG5qI](https://www.youtube.com/watch?v=H4VrKHVG5qI)
* [https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/](https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/)

