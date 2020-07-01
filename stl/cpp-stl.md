# Cpp Stl

## lower\_bound, upper\_bound, equal\_range

* `lower_bound`
  * Returns an iterator pointing to the first element in the range `[first,last)` which does not compare less than val. \(i.e. `>=`\).
  * The returned index `i` partitions the array `A` into two halves so that all elements in range `[first, i)` are `< x`, all elements in range `[i, last)` are `>= x`.
  * Same as `bisect.bisect_left` in python.
* `upper_bound`
  * Returns an iterator pointing to the first element in the range `[first,last)` which compares greater than val. \(i.e. `>`\).
  * The returned index `i` partitions the array `A` into two halves so that all elements in range `[first, i)` are `<= x`, all elements in range `[i, last)` are `> x`.
  * Same as `bisect.bisect_right` or `bisect.bisect` in python.

`equal_range`: Returns the bounds of the subrange that includes all the elements of the range `[first,last)` with values equivalent to val.

## set, map and their variants

`set`: The values are sorted.

`map`: The keys are sorted.

`multiset`: The values are sorted and duplicate values are allowed.

`multimap`: The keys are sorted and duplicate keys are allowed.

`unordered_set`, `unordered_map`, `unordered_multiset`, `unordered_multimap` are not sorted and organize their elements using hash tables. When you don't care about the ordering, use them. Their average time complexities are constants.

## Algorithm

### iota

https://en.cppreference.com/w/cpp/algorithm/iota

`iota`: Fills the range `[first, last)` with sequentially increasing values, starting with value and repetitively evaluating `++value`. Generating a sequentially increasing index array is one example use case as shown [here](https://leetcode.com/problems/maximum-profit-in-job-scheduling/discuss/409188/C%2B%2B-with-picture)

### next_permutation

`next_permutation`: Get the next permutation of a given input array. [Example use case](https://github.com/lzl124631x/LeetCode/tree/master/leetcode/556.%20Next%20Greater%20Element%20III)

### nth_element (Quick Select)

https://en.cppreference.com/w/cpp/algorithm/nth_element

`nth_element`: do a quick select such that the `n`-th element in the array is the `n`-th element in sorted order. All elements before it are less than or equal to it.

[Example use case](https://leetcode.com/problems/the-k-strongest-values-in-an-array/discuss/674384/C%2B%2BJavaPython-Two-Pointers-%2B-3-Bonuses)

Complexity: Linear in `std::distance(first, last)` on average.

### partial_sort

https://en.cppreference.com/w/cpp/algorithm/partial_sort

`partial_sort`: Rearranges elements such that the range `[first, middle)` contains the sorted `middle - first` smallest elements in the range `[first, last)`.

The order of equal elements is not guaranteed to be preserved. The order of the remaining elements in the range `[middle, last)` is unspecified.

[Example use case](https://leetcode.com/problems/the-k-strongest-values-in-an-array/discuss/674384/C%2B%2BJavaPython-Two-Pointers-%2B-3-Bonuses)

Complexity: Approximately `(last-first)log(middle-first)` applications of `cmp`

## Miscellaneous

### Why `unordered_set<pair<int, int>>` doesn't work while `set<pair<int, int>>` work?

`set` requires comparison operators. `pair<int, int>` has comparison operators defined, e.g. `<`.

`unordered_set` requires `hash` function being defined but `pair<int, int>` doesn't have that built-in definition.