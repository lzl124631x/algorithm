# Binary Search

778 793 1201 \(binary answer\)174

## Things to consider

### Search Range and Loop Condition

If it's an array, usually the search range is just `[0, N-1]` and the loop condition is `L <= R`.

### When and how to change the bounds

There should be a predicate separating the array in two parts. Use it as the condition.

For example, assume there is a `inLeft` function checking whether the current `M = (L + R) / 2` is in the left part.


```
// Initially
L                                          R
v                                          v
[      inLeft      ] [       !inLeft       ]

// Finally
                   R L
                   v v
[      inLeft      ] [       !inLeft       ]
```


```cpp
if (inLeft(M)) L = M + 1;
else R = M - 1;
```

### What's the result

As shown above, after the binary search, the `L` points to the first not-in-left-part element while `R` points to the last in-left-part element.

## Examples

### [275. H-Index II \(Medium\)](https://leetcode.com/problems/h-index-ii/)

The `inLeft` predicate is `A[M] < N - M` which is true for those citations DO NOT meet the h-index requirement. It splits the array into the following two parts.

```
// Initially
L                                                                       R
v                                                                       v
[     inLeft (A[M] < N - M)     ] [       !inLeft (A[M] >= N - M)       ]

// Finally
                                R L
                                v v
[     inLeft (A[M] < N - M)     ] [       !inLeft (A[M] >= N - M)       ]
```

If `inLeft(M)` then we should set `L = M + 1`, otherwise `R = M - 1`.

In the end, `L` points to the first in-the-right-part value, i.e. the first citation that is good to be included in the h-index, and the corresponding h-index is `N - L`.

```cpp
// OJ: https://leetcode.com/problems/h-index-ii/
// Author: github.com/lzl124631x
// Time: O(logN)
// Space: O(1)
class Solution {
public:
    int hIndex(vector<int>& A) {
        int N = A.size(), L = 0, R = N - 1;
        while (L <= R) {
            int M = (L + R) / 2;
            if (A[M] >= N - M) R = M - 1;
            else L = M + 1;
        }
        return N - L;
    }
};
```

### [1539. Kth Missing Positive Number (Easy)](https://leetcode.com/problems/kth-missing-positive-number/submissions/)

Let `L = 0, R = N - 1, M = (L + R) / 2`, and `f(M) = A[M] - M - 1` is the count of missing numbers in the subarray `A[0..M]`.

We'd like to find the last element whose `f(M) < k` which is the last element before the target missing number. Assume it's index is `t`, then `t + 1 + k` is the answer.

So we can define `isLeft(M)` as `f(M) < k`, and it will split the array into two parts:

```
A = [2,3,4,7,11], k = 5

A = [  2  3  4  7  11 ]
f = [  1  1  1  3   6 ]

Now split `A` according to `f`
                
               This is the one we're looking for
                v
                R      L
                v      v
A = [  2  3  4  7 ] [ 11 ]
f = [  1  1  1  3 ] [  6 ]

The index of 7 is 3, so the answer is 3 + 1 + k = 9
```

So the answer is `R + 1 + k` or `L + k`.

```cpp
// OJ: https://leetcode.com/problems/kth-missing-positive-number/
// Author: github.com/lzl124631x
// Time: O(logN)
// Space: O(1)
class Solution {
public:
    int findKthPositive(vector<int>& A, int k) {
        int L = 0, R = A.size() - 1;
        while (L <= R) {
            int M = (L + R) / 2;
            if (A[M] - M - 1 < k) L = M + 1;
            else R = M - 1;
        }
        return L + k;
    }
};
```

## Problems

**Problems that are good for practicing handwriting binary search**
* [704. Binary Search (Easy)](https://leetcode.com/problems/binary-search/)
* [275. H-Index II \(Medium\)](https://leetcode.com/problems/h-index-ii/)
* [1539. Kth Missing Positive Number (Easy)](https://leetcode.com/problems/kth-missing-positive-number/)
* [1044. Longest Duplicate Substring \(Hard\)](https://leetcode.com/problems/longest-duplicate-substring/)
* [668. Kth Smallest Number in Multiplication Table (Hard)](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/)

**Problems that are easier if we just use binary search library function**
* [300. Longest Increasing Subsequence (Medium)](https://leetcode.com/problems/longest-increasing-subsequence/)
* [497. Random Point in Non-overlapping Rectangles (Medium)](https://leetcode.com/problems/random-point-in-non-overlapping-rectangles/)

**Problems on arrays that are not sorted or monotonic**
* [162. Find Peak Element (Medium)](https://leetcode.com/problems/find-peak-element/)
* [33. Search in Rotated Sorted Array (Medium)](https://leetcode.com/problems/search-in-rotated-sorted-array/)
* [81. Search in Rotated Sorted Array II (Medium)](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

**Problems that I had to use `while (L < R)`**
* [153. Find Minimum in Rotated Sorted Array (Medium)](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
* [154. Find Minimum in Rotated Sorted Array II (Hard)](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)