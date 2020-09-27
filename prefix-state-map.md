# Prefix State Map

I haven't found an official name of this kind of problem. I just named it as "Prefix State Map".

This kind of problem is usually:

* about finding the longest/shortest subarray which satisfies some condition.
* the state of the subarray can be deduced using the the states of the prefix array. For example, to get the state of `A[i..j]`, you can use the state of two prefix array, `A[0..(i-1)]` and `A[0..j]`.

## Algorithm

Let `m` be a map from the state `state[i]` of a prefix subarray `A[0..i]` to the index of the first/last occurrence of the state `state[i]`.

For the current index `i` and its state `state[i]`, assume we want to get a subarray which ends at `i` and has a `target_state` and has the longest/shortest length.

Then we can use `state[i]` and `target_state` to compute a `prefix_state`. `j = m[prefix_state]` is the corresponding index of the prefix array. `A[(j+1) .. i]` is the longest/shortest optimal subarray.

## Pseudo Code

```cpp
int ans = 0; // the length of the optimal subarray
int state = 0; // assume the initial state is 0
unordered_map<int, int> m{{0, -1}}; // let -1 be the index of state `0`.
for (int i = 0; i < A.size(); ++i) {
    state = nextState(state, A[i]); // update state using A[i]
    int prefixState = getPrefixState(state); // based on problem description, we can compute a prefix state using the current state.
    ans = max(ans, i - m[prefixState]); // Use `min` if we are looking for the shortest subarray.
    m[state] = min(m[state], i); // Use `max` if we are looking for the shortest subarray.
}
```

## Problems

* [525. Contiguous Array \(Medium\)](https://leetcode.com/problems/contiguous-array/)
* [1371. Find the Longest Substring Containing Vowels in Even Counts \(Medium\)](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/)
* [1542. Find Longest Awesome Substring (Hard)](https://leetcode.com/problems/find-longest-awesome-substring/)
* [1590. Make Sum Divisible by P (Medium)](https://leetcode.com/problems/make-sum-divisible-by-p)