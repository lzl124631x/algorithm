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

**unordered** containers have greater initialization overhead compared to **ordered** counterparts. So don't create and destroy temporary **unordered** containers over and over again within loop which will significantly degrade the run time performance.

## array

If you know the length of the array, use `array<int, N>` where `N` is the fixed length (e.g. `array<int, 3>`), instead of `vector<int>`, because `array` will be more performant than `vector`.

One example problem is [1584. Min Cost to Connect All Points (Medium)](https://leetcode.com/problems/min-cost-to-connect-all-points/). If you use `vector<int>` to store an edge, the runtime is ~1500ms, and memory is 177MB while `array<int, 3>` only takes 500ms and 58MB.

## Algorithm

### iota

[https://en.cppreference.com/w/cpp/algorithm/iota](https://en.cppreference.com/w/cpp/algorithm/iota)

Fills the range `[first, last)` with sequentially increasing values, starting with value and repetitively evaluating `++value`. Generating a sequentially increasing index array is one example use case as shown [here](https://leetcode.com/problems/maximum-profit-in-job-scheduling/discuss/409188/C%2B%2B-with-picture)

### next\_permutation

Get the next permutation of a given input array. [Example use case](https://github.com/lzl124631x/LeetCode/tree/master/leetcode/556.%20Next%20Greater%20Element%20III)

### nth\_element \(Quick Select\)

[https://en.cppreference.com/w/cpp/algorithm/nth\_element](https://en.cppreference.com/w/cpp/algorithm/nth_element)

do a quick select such that the `n`-th element in the array is the `n`-th element (0-based) in sorted order. All elements before it are less than or equal to it.

Time complexity: `O(N)` on average, `O(NlogN)` in the worst case (Reference: https://en.wikipedia.org/wiki/Introselect)

Example use case:

* https://leetcode.com/problems/the-k-strongest-values-in-an-array/discuss/674384/C%2B%2BJavaPython-Two-Pointers-%2B-3-Bonuses
* [215. Kth Largest Element in an Array (Medium)](https://leetcode.com/problems/kth-largest-element-in-an-array/)

Take [215. Kth Largest Element in an Array (Medium)](https://leetcode.com/problems/kth-largest-element-in-an-array/) for example, we are looking for the first `0 ~ k - 1`th element that are **greater** than other elements.

```cpp
// OJ: https://leetcode.com/problems/kth-largest-element-in-an-array/
// Author: github.com/lzl124631x
// Time: O(N) on average, O(N^2) in the worst case
// Space: O(1)
class Solution {
public:
    int findKthLargest(vector<int>& A, int k) {
        nth_element(begin(A), begin(A) + k - 1, end(A), greater<int>());
        return A[k - 1];
    }
};
```

Note: [Difference between `nth_element` and `partial_sort`](https://stackoverflow.com/a/54227430/3127828)

### partial\_sort

[https://en.cppreference.com/w/cpp/algorithm/partial\_sort](https://en.cppreference.com/w/cpp/algorithm/partial_sort)

Rearranges elements such that the range `[first, middle)` contains the sorted `middle - first` smallest elements in the range `[first, last)`.

The order of equal elements is not guaranteed to be preserved. The order of the remaining elements in the range `[middle, last)` is unspecified.

[Example use case](https://leetcode.com/problems/the-k-strongest-values-in-an-array/discuss/674384/C%2B%2BJavaPython-Two-Pointers-%2B-3-Bonuses)

Time Complexity: `O(NlogK)`

### partial\_sum

[https://en.cppreference.com/w/cpp/algorithm/partial\_sum](https://en.cppreference.com/w/cpp/algorithm/partial_sum)

```cpp
partial_sum(inFirst, inLast, outFirst)
```

Computes the partial sums of the elements in the subranges of the range `[inFirst, inLast)` and writes them to the range beginning at `outFirst`.

```cpp
// Generate a `pre` array such that `pre[j + 1] - pre[i]` = sum of numbers between i and j.
int N = A.size();
vector<int> pre(N + 1);
partial_sum(begin(A), end(A), begin(pre) + 1);
// Or simply
for (int i = 0; i < N; ++i) pre[i + 1] = pre[i] + A[i];
```

### unique

[https://en.cppreference.com/w/cpp/algorithm/unique](https://en.cppreference.com/w/cpp/algorithm/unique)

`unique` function removes **consecutive** duplicate elements. So if we want to get the uniqueness of the elements in the entire container, use `sort` first.

```cpp
// v is a vector<int>
sort(begin(v), end(v));
v.resize(unique(begin(v), end(v)) - begin(v));
```

### next_permutation

[https://en.cppreference.com/w/cpp/algorithm/next_permutation](https://en.cppreference.com/w/cpp/algorithm/next_permutation)

Permutes the range `[first, last)` into the next permutation, where the set of all permutations is ordered lexicographically with respect to `operator<` or `comp`. Returns true if such a "next permutation" exists; otherwise transforms the range into the lexicographically first permutation (as if by `std::sort(first, last, comp)`) and returns false.

```cpp
string s = "aba";
sort(s.begin(), s.end());
do {
    cout << s << endl;
} while (next_permutation(s.begin(), s.end()));
/*
aab
aba
baa
*/
```

## Miscellaneous

### Why `unordered_set<pair<int, int>>` doesn't work while `set<pair<int, int>>` work?

`set` requires comparison operators. `pair<int, int>` has comparison operators defined, e.g. `<`.

`unordered_set` requires `hash` function being defined but `pair<int, int>` doesn't have that built-in definition.

### How to iterate `multiset` or `multimap`?

Use `equal_range`.

```cpp
multiset<int> s;
s.insert(1);
s.insert(2);
s.insert(1);
for (auto i = s.begin(); i != s.end();) {
    auto range = s.equal_range(*i);
    for (auto j = range.first; j != range.second; ++j) {
        cout << *j << endl;
    }
    i = range.second;
}
/*
1
1
2
*/
```

```cpp
multimap<int, int> m;
m.emplace(1, 1);
m.emplace(1, 2);
m.emplace(2, 3);
for (auto i = m.begin(); i != m.end();) {
    auto range = m.equal_range(i->first);
    for (auto j = range.first; j != range.second; ++j) {
        cout << j->first << " " << j->second << endl;
    }
    i = range.second;
}
/*
1 1
1 2
2 3
*/
```

### Erase map element during traversal

```cpp
for (auto it = m.begin(); it != m.end();) {
    if (shouldRemove(it)) it = m.erase(it);
    else ++it;
}
```