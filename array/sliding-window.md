# Sliding Window

## Find Maximum Window

See my post [C++ Maximum Sliding Window Cheatsheet Template!](https://leetcode.com/problems/frequency-of-the-most-frequent-element/discuss/1175088/C%2B%2B-Maximum-Sliding-Window-Cheatsheet-Template!)

### Template 1: Shrinkable Sliding Window

```cpp
int i = 0, j = 0, ans = 0;
for (; j < N; ++j) {
    // CODE: use A[j] to update state which might make the window invalid
    for (; invalid(); ++i) { // when invalid, keep shrinking the left edge until it's valid again
        // CODE: update state using A[i]
    }
    ans = max(ans, j - i + 1); // the window [i, j] is the maximum window we've found thus far
}
return ans;
```

Essentially, we want to **keep the window valid** at the end of each outer `for` loop.

### Template 2: Non-shrinkable Sliding Window

```cpp
int i = 0, j = 0;
for (; j < N; ++j) {
    // CODE: use A[j] to update state which might make the window invalid
    if (invalid()) { // Increment the left edge ONLY when the window is invalid. In this way, the window GROWs when it's valid, and SHIFTs when it's invalid
        // CODE: update state using A[i]
        ++i;
    }
    // after `++j` in the for loop, this window `[i, j)` of length `j - i` MIGHT be valid.
}
return j - i; // There must be a maximum window of size `j - i`.
```

Essentially, we GROW the window when it's valid, and SHIFT the window when it's invalid.

Note that there is only a SINGLE `for` loop now!

## Find Minimum Window

```cpp
int i = 0;
for (int j = 0; j < N; ++j) {
    // use A[j] to update state.
    while (valid()) {
        ans = min(ans, j - i + 1);
        // use A[i] to update state
        ++i;
    }
}
```

## Problems

Find Minimum

* [76. Minimum Window Substring \(Hard\)](https://leetcode.com/problems/minimum-window-substring/)
* [209. Minimum Size Subarray Sum \(Medium\)](https://leetcode.com/problems/minimum-size-subarray-sum/)

Find Maximum

* [3. Longest Substring Without Repeating Characters \(Medium\)](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
* [159. Longest Substring with At Most Two Distinct Characters \(Hard\)](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)
* [340. Longest Substring with At Most K Distinct Characters \(Hard\)](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
* [424. Longest Repeating Character Replacement (Medium)](https://leetcode.com/problems/longest-repeating-character-replacement/)
* [487. Max Consecutive Ones II (Medium)](https://leetcode.com/problems/max-consecutive-ones-ii/)
* [713. Subarray Product Less Than K (Medium)](https://leetcode.com/problems/subarray-product-less-than-k/)
* [930. Binary Subarrays With Sum (Medium)](https://leetcode.com/problems/binary-subarrays-with-sum/)
* [992. Subarrays with K Different Integers (Hard)](https://leetcode.com/problems/subarrays-with-k-different-integers/)
* [1004. Max Consecutive Ones III (Medium)](https://leetcode.com/problems/max-consecutive-ones-iii/)
* [1208. Get Equal Substrings Within Budget (Medium)](https://leetcode.com/problems/get-equal-substrings-within-budget/)
* [1248. Count Number of Nice Subarrays (Medium)](https://leetcode.com/problems/count-number-of-nice-subarrays/)
* [1493. Longest Subarray of 1's After Deleting One Element \(Medium\)](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)
* [1658. Minimum Operations to Reduce X to Zero (Medium)](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/)
* [1695. Maximum Erasure Value (Medium)](https://leetcode.com/problems/maximum-erasure-value/)
* [1838. Frequency of the Most Frequent Element (Medium)](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)
* [2009. Minimum Number of Operations to Make Array Continuous (Hard)](https://leetcode.com/problems/minimum-number-of-operations-to-make-array-continuous/)
* [2024. Maximize the Confusion of an Exam (Medium)](https://leetcode.com/problems/maximize-the-confusion-of-an-exam/)

Find Minimum and Maximum

* [992. Subarrays with K Different Integers (Hard)](https://leetcode.com/problems/subarrays-with-k-different-integers/)

Fixed Window Size

* [1461. Check If a String Contains All Binary Codes of Size K \(Medium\)](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)
