# Difference Array

Assume we have an array `A` of size `N`, and given a list of updates of length `K`, each of which is in the form of `[from, to, diff]` which means adding `diff` to all elements with indexes in range `[from, to]`.

For example:

```
A: [0, 0, 0, 0]
updates: [[0,1,2], [1,2,1],[3,3,6]]

// after updates
A: [2, 3, 1, 6]
```

The brute force way is to visit all elements in range `[from, to]` and add `diff` to them for each update. It will take `O(NK)` time.

We can improve this by using difference array, i.e. only storing the delta value at break points. Let `delta[i]` be `U[i] - U[i-1]` where `U[i]` is the sum of all `diff`s to `A[i]`.

```
A: [0, 0, 0, 0]
updates: [[0,1,2], [1,2,1],[3,3,6]]

U: [2, 3, 1, 6]
delta: [2, 1, -2, 5]
// after update
A: [2, 2+1, 2+1-2, 2+1-2+5] = [2, 3, 1, 6]
```

For `update[i] = [from, to, diff]`, we do:

* `delta[from] += diff`
* `delta[to+1] -= diff`

```
updates: [[0,1,2], [1,2,1],[3,3,6]]
// apply first update
delta: [2, 0, -2, 0]
// apply second update
delta: [2, 1, -2, -1]
// apply third update
delta: [2, 1, -2, 5]
```

So in the end we just need to traverse `delta` array to recover the `U` array.

## Problems

* [1674. Minimum Moves to Make Array Complementary (Medium)](https://leetcode.com/problems/minimum-moves-to-make-array-complementary/)
* [1854. Maximum Population Year (Easy)](https://leetcode.com/problems/maximum-population-year/)