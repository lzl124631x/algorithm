# Rabin Karp

## Rabin Karp

A string-searching algorithm that uses rolling hash to find an exact match of a pattern string in a text.

The expected time complexity is `O(S+T)` where `S` and `T` are the lengths of the original string `s` and the pattern string `t` respectively.

Its worst-case time complexity is `O(ST)`.

### Algorithm

First get the hash `ht` of the pattern string `t`.

We keep a sliding window of size `T` and compute the hash `hs` of the substring of `s` within the window.

Whenever `ht == hs`, the part in the window might be the same as `t`. In case of hash conflict, we linearly check the equity of them.

### Implementation

#### Version 1

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

#### Version 2

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

#### Version 3

* The hashes are `unsigned long long`. Using `unsigned long long` can **reduce** the chance of hash conflict. (Example:  [1923. Longest Common Subpath (Hard)](https://leetcode.com/problems/longest-common-subpath/))
* No mod number `m` is needed! It will automatically mod by `2^32` when overflow happens.

```cpp
// OJ: https://leetcode.com/problems/implement-strstr/
// Author: github.com/lzl124631x
// Time: average O(S + T), worst O(ST)
// Space: O(1)
class Solution {
public:
    int strStr(string s, string t) {
        unsigned long long S = s.size(), T = t.size(), d = 1099511628211, p = 1, hs = 0, ht = 0; // we can use d = 29 as well or some other prime greater than the size of the character set.
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

#### Why 1099511628211?

`1099511628211` is the FNV(Fowler–Noll–Vo) prime for 64 bit integer. When using FNV prime, the hash values produced are more scattered throughout the hash space.

```
// Ref: https://www.jianshu.com/p/da951d62bb67
32 bit FNV_prime = 224 + 28 + 0x93 = 16777619

64 bit FNV_prime = 240 + 28 + 0xb3 = 1099511628211

128 bit FNV_prime = 288 + 28 + 0x3b = 309485009821345068724781371

256 bit FNV_prime = 2168 + 28 + 0x63 = 374144419156711147060143317175368453031918731002211

512 bit FNV_prime = 2344 + 28 + 0x57 =
35835915874844867368919076489095108449946327955754392558399825615420669938882575
126094039892345713852759

1024 bit FNV_prime = 2680 + 28 + 0x8d =
50164565101131186554345988110352789550307653454047907443030175238311120551081474
51509157692220295382716162651878526895249385292291816524375083746691371804094271
873160484737966720260389217684476157468082573
```

See more in [FNV prime](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo\_hash\_function#FNV\_prime)

### Application

Rabin Karp algorithm or rolling hash is often used in problems where you need to find repeated subarray/substring in a long array/string.

Examples:

* [718. Maximum Length of Repeated Subarray (Medium)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)
* [1044. Longest Duplicate Substring (Hard)](https://leetcode.com/problems/longest-duplicate-substring/)

The following function is used in [718. Maximum Length of Repeated Subarray (Medium)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/) to generate the rolling hash.

```cpp
vector<ULL> rolling(vector<int> &A, int len) {
    unsigned long long d = 1099511628211, h = 0, p = 1;
    vector<ULL> ans;
    for (int i = 0; i < A.size(); ++i) {
        h = h * d + A[i];
        if (i < len) p *= d;
        else h -= A[i - len] * p;
        if (i >= len - 1) ans.push_back(h);
    }
    return ans;
}
```

### Prefix Hash Array

```cpp
// Generate Prefix Hash Array
unsigned long long d = 1099511628211, h[501] = {}, p[501] = {1}, N = s.size();
for (int i = 0; i < N; ++i) {
    p[i + 1] = p[i] * d;
    h[i + 1] = h[i] * d + s[i];
}
// Access a hash of a substring of s[begin, end)
unsigned long long hash(int begin, int end) {
    return h[end] - h[begin] * p[end - begin];
}
```

Example: [1698. Number of Distinct Substrings in a String (Medium)](https://leetcode.com/problems/number-of-distinct-substrings-in-a-string/)

```cpp
// OJ: https://leetcode.com/problems/number-of-distinct-substrings-in-a-string/
// Author: github.com/lzl124631x
// Time: O(N^2)
// Space: O(N^2)
class Solution {
    unsigned long long d = 1099511628211, h[501] = {}, p[501] = {1};
    unsigned long long hash(int begin, int end) {
        return h[end] - h[begin] * p[end - begin];
    }
public:
    int countDistinct(string s) {
        int N = s.size();
        for (int i = 0; i < N; ++i) {
            p[i + 1] = p[i] * d;
            h[i + 1] = h[i] * d + s[i];
        }
        unordered_set<unsigned long long> seen;
        for (int len = 1; len <= N; ++len) {
            for (int i = 0; i + len <= N; ++i) {
                seen.insert(hash(i, i + len));
            }
        }
        return seen.size();
    }
};
```

### Problem

* [28. Implement strStr() (Easy)](https://leetcode.com/problems/implement-strstr/)
* [1044. Longest Duplicate Substring (Hard)](https://leetcode.com/problems/longest-duplicate-substring/)
* [718. Maximum Length of Repeated Subarray (Medium)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)
* [214. Shortest Palindrome (Hard)](https://leetcode.com/problems/shortest-palindrome/)
* [1297. Maximum Number of Occurrences of a Substring (Medium)](https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring/)
* [1923. Longest Common Subpath (Hard)](https://leetcode.com/problems/longest-common-subpath/)
* [1062. Longest Repeating Substring (Medium)](https://leetcode.com/problems/longest-repeating-substring/)
* [718. Maximum Length of Repeated Subarray (Medium)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)
* [1698. Number of Distinct Substrings in a String (Medium)](https://leetcode.com/problems/number-of-distinct-substrings-in-a-string/)

### Reference

* [https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm)
* [https://www.youtube.com/watch?v=H4VrKHVG5qI](https://www.youtube.com/watch?v=H4VrKHVG5qI)
* [https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/](https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/)

## TODO

[https://codeforces.com/blog/entry/4898](https://codeforces.com/blog/entry/4898)
