# Binary Answer

On a small scale of input, the binary answer problems are solvable by linear scaning the answers. When the input scale is large, we need to use binary answer to reduce the scaning complexity from linear `O(N)` to binary `O(logN)`.

Binary search the answer in a known range. Assume the range is `[L, R]`, and the current value we are checking is `M = (L + R) / 2`.

We can use "Binary Answer" solution if we can write a predicate function `valid(M)` that has **monotocity**:

* If `valid(i) == true`, then `valid(j) == true` for all `j <= i`.
* If `valid(i) == false`, then `valid(j) == false` for all `j >= i`.

![](.gitbook/assets/binary-answer-1.png)

Our goal is the find the maximum `i` that `valid(i) == true`.

We can use two pointers `L = minVal, R = maxVal`, and keep using binary search to move the pointers towards each other until they swap order.

![](.gitbook/assets/binary-answer-2.png)

## Pseudo Code

Assume the answer is monotonically descreasing \(from valid to invalid\), and we are looking for the maximum valid value.

```cpp
int L = minVal, R = maxVal
while (L <= R) {
    int M = (L + R) / 2;
    if (valid(M)) L = M + 1;  // You can also `ans = L` here to store the answer
    else R = M - 1;
}
return R >= minVal ? R : NOT_FOUND;
```

## Problems

* [410. Split Array Largest Sum \(Hard\)](https://leetcode.com/problems/split-array-largest-sum/)
* [1482. Minimum Number of Days to Make m Bouquets \(Medium\)](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)
* [1300. Sum of Mutated Array Closest to Target \(Medium\)](https://leetcode.com/problems/sum-of-mutated-array-closest-to-target/)
* [1044. Longest Duplicate Substring \(Hard\)](https://leetcode.com/problems/longest-duplicate-substring/)
* [668. Kth Smallest Number in Multiplication Table (Hard)](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/)
* [719. Find K-th Smallest Pair Distance (Hard)](https://leetcode.com/problems/find-k-th-smallest-pair-distance/)
* [1283. Find the Smallest Divisor Given a Threshold (Medium)](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/)
* [1802. Maximum Value at a Given Index in a Bounded Array (Medium)](https://leetcode.com/problems/maximum-value-at-a-given-index-in-a-bounded-array/)
* [1870. Minimum Speed to Arrive on Time (Medium)](https://leetcode.com/problems/minimum-speed-to-arrive-on-time/)
* [1648. Sell Diminishing-Valued Colored Balls (Medium)](https://leetcode.com/problems/sell-diminishing-valued-colored-balls/)