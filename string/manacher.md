# Manacher's Algorithm - Finding all sub-palindromes in `O(N)` time

## Algorithm

### Step 1. Inserting special characters

Assume the original string is `s`, and the string after insertion is `t`.

We insert special characters into `s` so as to avoid handling palindromes of odd length and even length separately.

We insert `*` around every character, prepend a `^` and append a `$` to the string.

For example:

```cpp
// Odd case
"aba" // the palindrome was of length 3
// becomes
"^*a*b*a*$" // the palindrome is of length 7
//   ^
//   this b is still the center.


// Even case
"abba" // the palindrome was of length 4
// becomes
"^*a*b*b*a*$" // the palindrome is of length 9
//    ^
//    this * becomes the new center
```

**How to convert the indexes between `s` and `t`?**

```
      0 1 2
s =   a b c
    012345678
t = ^*a*b*c*$
```

```cpp
(indexInS + 1) * 2 = indexInT

indexInT / 2 - 1 = indexInS
```

We only need to consider the indexes in `t` in range `[2, s.size() * 2]`, which corresponds to `[0, s.size()-1]` in `s`.

### Step 2. Calculate `r` array

Let `r[i]` be the number of palindromes centered at `t[i]` (aka. the radius of the longest palindrome centered at `t[i]`).

So we have:
* the longest palindrome centered at `t[i]` is `t[i-r[i]+1 .. i+r[i]-1]`.
* For any `k < r[i]`, substring `t[i-k .. i+k]` is a palindrome.
* For any `k >= r[i]`, substring `t[i-k .. i+k]` is not a palindrome.

Example:

```cpp
     0123456789012
t = "^*a*b*c*b*d*$"
        ^  ^  ^
```
For center `t[6] = 'c'`, we have `r[6] = 4`:
* The longest palindrome centered at `t[6]` is `t[3 .. 9] = "*b*c*b*"`
* For any `k < 4`, substring `t[6-k .. 6+k]` is a palindrome. E.g. for `k = 2`, `t[4 .. 8] = "b*c*b"` is a palindrome.
* For any `k >= 4`, substring `t[6-k .. 6+k]` is not a palindrome. E.g. for `k = 4`, `t[2 .. 10] = "a*b*c*b*d"` is not a palindrome.

To calculate `r[i]` efficiently, we need to **leverage the computed `r[j]` (`j < i`) values.

Let `j < i` be the center with the furthest reach `j + r[j]`.

For `i-1`, since `r[i-1] >= 1`, the corresponding reach `i-1 + r[i-1]` is at least `i`.

So, `j + r[j]` must be `>= i`.


**Case 1: j + r[j] == i**:

Since the palindrome `t[j-r[j]+1 .. j+r[j]-1]` doesn't cover `t[i]`, we have no information to leverage, and have to expand from `t[i]` in a brute force way.

Example: 

```cpp
     01234567890
t = "^*a*b*c*b*$"
        [^]^
         j i
```

For `t[6] = 'c'`, the corresponding `j` could be `4` and `r[j] = 2`. Since `j + r[j] == i == 6`, we expand at `t[6]` brute-forcely. And we will get `r[6] = 4`.

Since `i + r[i] = 6 + 4 = 10 > j + r[j] = 6`, we will make `i = 6` be the new center `j`.

**Case 2: j + r[j] > i**:

Now we can leverage some symmetry information.

Assume the symmetry point of `i` relative to `j` is `k = 2*j-i`. We need to consider 3 subcases about whether the range `(j-r[j], j+r[j])` covers `(k-r[k], k+r[k])`.

**Case 2a: `k-r[k] > j-r[j]`**:

Example:

```cpp
     012345678901234
t = "$*a*b*c*b*a*d*$"
         ^ ^ ^
         k j i
r =    212161?
      [    j    ]
        [k] [i]
```

In this case, we must have `r[i] = r[k] = r[2*j-i]` due to symmetry.

**Case 2b: `k-r[k] < j-r[j]`**:

Example:

```cpp
     01234567890123456789012
t = "^*c*a*a*c*c*c*a*a*d*b*$"
          ^    ^    ^
          k    j    i
r =    2125212383212?
        [      j      ]
      [   k   ]          // not all this range for `k` can be symmetrised.
        [ k ]     [ i ]  // we can only symmetrise part covered by the range for `j`
```

Since `k-r[k] < j-r[j]`, not the entire range for `k` can be symmetrised relative to `j`. We can only symmetrise the range `[j-r[j]+1, 2*k-j+r[j]-1]`. So `r[i]` is at least `k - (j-r[j]+1) + 1 = k-j+r[j] = j+r[j]-i`.

Assume:
* the range for `j` is surrounded by index `p` and `s`.
* `p` and `q` are symmetric relative to `k`.
* `r` and `s` are symmetric relative to `i`.

```
       p[      j      ]s
       p[ k ]q   r[ i ]s
```

We know:

* `t[p] != t[s]` and `t[q] == t[r]` because of `r[j]`.
* `t[p] == t[q]` because of `r[k]`.

So, `t[p] == t[q] == t[r] != t[s]`. Thus, `r[i]` is exactly `j+r[j]-i`.

**Case 2c: `k-r[k] == j-r[j]`**:

Example:

```cpp
     01234567890123456789012
t = "^*d*a*a*c*c*c*a*a*c*b*$"
          ^    ^    ^
          k    j    i
r =    2123212383212?
       p[      j      ]s
       p[ k ]q   r[ i ]s
```

This is similar to case 2b that we know `r[i]` is at least `j+r[j]-i`, with the exception that `t[p] != t[q]` now.

Given `t[p] != t[s]`, `t[q] == t[r]` and `t[p] != t[q]`, we can't know whether `t[r] == t[s]` or not. 

So, we have to do brute force expansion with `r[i]` being at least `j+r[j]-i`.

**Summary of the cases**:

* Case 1: `j + r[j] == i`, we do brute force expansion starting from `r[i] = 1`.
* Case 2: `j + r[j] > i`.
  * Case 2a: `k-r[k] > j-r[j]`, we have `r[i] = r[k] = r[2*j-i] < j+r[j]-i`
  * Case 2b: `k-r[k] < j-r[j]`, we have `r[i] = j+r[j]-i < r[k]`.
  * Case 2c: `k-r[k] == j-r[j]`, we do brute force expansion starting from `r[i] = j+r[j]-i = r[k]`.

Note that for the 3 subcases in case 2, `r[i]` is either exactly or at least the minimum of `r[2*j-i]` and `j+r[j]-i`.

**Merging the cases**:

We don't need to implement these cases separately. Instead, we use the following implementation to cover all the cases.

```cpp
vector<int> r(M); // `M` is the length of `t`.
r[1] = 1;
int j = 1; 
for (int i = 2; i <= 2 * N; ++i) {
    // The current radius `cur` is `r[i]`.
    // For case 1, `r[i]` starts from 1.
    // For case 2, `r[i]` starts from `min(r[2*j-i], j+r[j]-i)` which satisfies all the 3 subcases.
    int cur = j + r[j] > i ? min(r[2 * j - i], j + r[j] - i) : 1;
    while (t[i - cur] == t[i + cur]) ++cur; // expanding the current radius
    if (i + cur > j + r[j]) j = i; // if `i` has further reach, make `i` the new `j`.
    r[i] = cur;
}
```

### Step 3. Get the longest palindromic substrings in `s`

**Get the length**:

The length of the longest palindrome centering at `t[i]` is `i+r[i]-(i-r[i])-1 = 2*r[i]-1`.

The edges of this palindrome must be `*`. The number of `*` must be 1 plus the number of valid characters.

Thus, the length of the longest palindrome corresponding to `r[i]` must be `r[i]-1`.

**Get the starting index**:

The longest palindrome centering at `t[i]` starts at index `i - floor((2*r[i] - 1) / 2)`. Since `2*r[i]-1` must be an odd number, `i - floor((2*r[i] - 1) / 2) = i - (2*r[i] - 2) / 2 = i - r[i] + 1`.

Since the palindrome must start with `*`, the first valid character starts at `i - r[i] + 2`.

Based on the aforementioned equation `indexInT / 2 - 1 = indexInS`, the palindrome starts at `(i - r[i] + 2) / 2 - 1 = (i - r[i]) / 2` in `s`.

## Implementation

```cpp
// OJ: https://leetcode.com/problems/longest-palindromic-substring/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(N)
class Solution {
public:
    string longestPalindrome(string s) {
        int N = s.size();
        string t = "^*";
        for (char c : s) {
            t += c;
            t += '*';
        }
        t += '$'; // inflating the `s` ( example: "abc" becomes "^*a*b*c*$" )
        int M = t.size();
        
        vector<int> r(M); // `r[i]` is the number of palindromes with `t[i]` as the center (aka. the radius of the longest palindrome centered at `t[i]`)
        r[1] = 1;
        int j = 1; // `j` is the index with the furthest reach `j + r[j]`
        for (int i = 2; i <= 2 * N; ++i) {
            int cur = j + r[j] > i ? min(r[2 * j - i], j + r[j] - i) : 1; // `t[2*j-i]` is the symmetry point to `t[i]`
            while (t[i - cur] == t[i + cur]) ++cur; // expanding the current radius
            if (i + cur > j + r[j]) j = i;
            r[i] = cur;
        }
        
        int mx = 0, p = -1;
        for (int i = 2; i <= 2 * N; ++i) {
            if (r[i] - 1 > mx) {
                mx = r[i] - 1;
                p = (i - r[i]) / 2;
            }
        }
        return s.substr(p, mx);
    }
};
```

## Time Complexity

It seems like that we might do brute force expansion at every `i` so the time complexity is `O(N^2)`, but actually we won't do that many expansions.

Whenever we successfully do an expansion with `k` steps, we will update `j` increasing `j + r[j]` by `k`. So, the amount of expansion is the same as the increase to `j + r[j]`. Since `j + r[j]` at most increases by `t.size()`, the amount of expansion we did must be at most `t.size()` times, which takes `O(N)` time.

Thus, the overall time complexity of Manacher is `O(N)`.

## Reference

* [[Algorithm][018] 最长回文子串 Manacher [OTTFF]](https://www.bilibili.com/video/BV1AX4y1F79W)