# Rabin Karp

A string-searching algorithm that uses rolling hash to find an exact match of a pattern string in a text.

The expected time complexity is `O(S+T)` where `S` and `T` are the lengths of the original string `s` and the pattern string `t` respectively.

Its worst-case time complexity is `O(ST)`.

## Algorithm

First get the hash `ht` of the pattern string `t`.

We keep a sliding window of size `T` and compute the hash `hs` of the substring of `s` within the window.

Whenever `ht == hs`, the part in the window might be the same as `t`. In case of hash conflict, we linearly check the equity of them.

## Implementation

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

The following function is used in [718. Maximum Length of Repeated Subarray (Medium)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/) to generate the rolling hash.

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
* [1044. Longest Duplicate Substring (Hard)](https://leetcode.com/problems/longest-duplicate-substring/)
* [718. Maximum Length of Repeated Subarray (Medium)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

## Reference

* [https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm)
* [https://www.youtube.com/watch?v=H4VrKHVG5qI](https://www.youtube.com/watch?v=H4VrKHVG5qI)
* [https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/](https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/)

