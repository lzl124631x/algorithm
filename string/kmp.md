# KMP

KMP \(Knuth-Morris-Pratt\) algorithm is a substring search algorithm.

LPS: Longest proper Prefix which is also Suffix. For example, `"abcdeabc"` has the LPS `"abc"`. Note that the string itself can't be an LPS of itself.

## Algorithm

1. Generate the LPS array.
2. Take advantage of the LPS array so that once we find a mismatch, we can efficiently jump to an earlier point and continue searching.

For the pattern string `t` we are searching, the `lps` array is of the same length as `t`. `lps[i]` is the length of LPS of `t[0..i]`.

Example:

```
t   = a b a b x a b a b x g
lps = 0 0 1 2 0 1 2 3 4 5 0
```

## Implementation

```cpp
// OJ: https://leetcode.com/problems/implement-strstr/
// Author: github.com/lzl124631x
// Time: O(M+N)
// Space: O(N)
class Solution {
    vector<int> getLps(string &s) {
        int N = s.size(), i = 0;
        vector<int> lps(N); // lps[0] is always 0
        for (int j = 1; j < N; ++j) {
            while (i > 0 && s[i] != s[j]) i = lps[i - 1]; // when s[i] can't match s[j], keep moving `i` backwards to `lps[i-1]`
            i += s[i] == s[j]; // Move `i` if s[j] matches s[i]
            lps[j] = i; // Assign the matched length to lps[j]
        }
        return lps;
    }
public:
    int strStr(string s, string t) {
        if (t.empty()) return 0;
        int M = s.size(), N = t.size(), i = 0, j = 0;
        auto lps = getLps(t);
        while (i < M) {
            if (s[i] == t[j]) { // if match, move i and j once
                ++i;
                ++j;
                if (j == N) return i - N; // Full match found. 
            } else {
                if (j) j = lps[j - 1]; // The first lps[j-1] charaters are already matched. Start the next match from lps[j-1]
                else ++i; // The first character is not matched, move i.
            }
        }
        return -1;
    }
};
```

## Problems

* [28. Implement strStr\(\) \(Easy\)](https://leetcode.com/problems/implement-strstr/)
* [214. Shortest Palindrome \(Hard\)](https://leetcode.com/problems/shortest-palindrome/)
* [1392. Longest Happy Prefix \(Hard\)](https://leetcode.com/problems/longest-happy-prefix/)
* [459. Repeated Substring Pattern (Easy)](https://leetcode.com/problems/repeated-substring-pattern/)


## KMP Count

Count the occurrence of `t` in string `s`.

```cpp
// OJ: https://leetcode.com/problems/number-of-subarrays-that-match-a-pattern-ii
// Author: github.com/lzl124631x
// Time: O(M + N)
// Space: O(M + N)
class Solution {
    vector<int> getLps(string &s) {
        int N = s.size(), i = 0;
        vector<int> lps(N); // lps[0] is always 0
        for (int j = 1; j < N; ++j) {
            while (i > 0 && s[i] != s[j]) i = lps[i - 1]; // when s[i] can't match s[j], keep moving `i` backwards to `lps[i-1]`
            i += s[i] == s[j]; // Move `i` if s[j] matches s[i]
            lps[j] = i; // Assign the matched length to lps[j]
        }
        return lps;
    }
    int KMPCount(string s, string t) {
        if (t.empty()) return 0;
        int M = s.size(), N = t.size(), i = 0, j = 0, ans = 0;
        auto lps = getLps(t);
        while (i < M) {
            if (s[i] == t[j]) {
                ++i;
                ++j;
                if (j == N) {
                    ++ans;
                    j = lps[j - 1]; // Full match found. Move `j` back to `lps[j-1]` to skip matched prefix
                }
            } else {
                if (j) j = lps[j - 1];
                else ++i;
            }
        }
        return ans;
    }
public:
    int countMatchingSubarrays(vector<int>& A, vector<int>& P) {
        int N = A.size(), M = P.size();
        string s, p;
        for (int i = 1; i < N; ++i) {
            s.push_back(A[i] > A[i - 1] ? 'a' : (A[i] == A[i - 1] ? 'b' : 'c'));
        }
        for (auto &n : P) {
            p.push_back(n == 1 ? 'a' : (n == 0 ? 'b' : 'c'));
        }
        return KMPCount(s, p);
    }
};
```

## Problems
* [3036. Number of Subarrays That Match a Pattern II (Hard)](https://leetcode.com/problems/number-of-subarrays-that-match-a-pattern-ii)

## Reference

* [https://youtu.be/GTJr8OvyEVQ](https://youtu.be/GTJr8OvyEVQ)
* [https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/](https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/)

