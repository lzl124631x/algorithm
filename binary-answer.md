# Binary Answer

On a small scale of input, the binary answer problems are solvable by linear scaning the answers. When the input scale is large, we need to use binary answer to reduce the scaning complexity from linear `O(N)` to binary `O(logN)`.

Binary search the answer in a known range. Assume the range is `[L, R]`, and the current value we are checking is `M = (L + R) / 2`.

If we can write a function `valid(M)` to check if `M` is valid, and the problem satisfies the following monotone requirement:
* If `i` is **valid**, then for all `k < i`, `k` is **valid**
* If `j` is **invalid**, then for all `k > j`, `k` is **invalid**

Then we can use binary answer.

```
[                  Answer Range                  ]
[       Valid Range    ][      Invalid Range     ]
                       ^
binary search to find  ┘
```

## Problems

* [410. Split Array Largest Sum \(Hard\)](https://leetcode.com/problems/split-array-largest-sum/)
* [1482. Minimum Number of Days to Make m Bouquets (Medium)](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)
* [1300. Sum of Mutated Array Closest to Target (Medium)](https://leetcode.com/problems/sum-of-mutated-array-closest-to-target/)
* [1044. Longest Duplicate Substring (Hard)](https://leetcode.com/problems/longest-duplicate-substring/)