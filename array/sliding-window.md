# Sliding Window

## Tip 1

[Here is a 10-line template that can solve most 'substring' problems](https://leetcode.com/problems/minimum-window-substring/discuss/26808/here-is-a-10-line-template-that-can-solve-most-substring-problems)

Template:

```cpp
int findSubstring(string s){
    vector<int> map(128,0);
    int counter; // check whether the substring is valid
    int begin=0, end=0; //two pointers, one point to tail and one  head
    int d; //the length of substring

    for() {
        /* initialize the hash map here */ 
    }

    while(end<s.size()) {
        if(map[s[end++]]-- /* ? */) {
            /* modify counter here */
        }
        while(/* counter condition */) { 
            /* update d here if finding minimum*/
            /* increase begin to make it invalid/valid again */
            if(map[s[begin++]]++ /* ? */) {
                /*modify counter here*/
            }
        }  
        /* update d here if finding maximum*/
    }
    return d;
}
```

When finding minimum:

* this template works when initially counter condition is invalid, so we are extending the window to find the first valid window.
* the counter condition should be `true` for **valid** window, i.e. shrinking the valid window to find the minimum.

When finding maximum:

* this template works when initially counter condition is valid, so we are extending valid window as large as possible until it becomes invalid.
* the counter condition should be `true` for **invalid** window, i.e. shrinking the invalid window until it becomes valid again.

## Find Maximum Window

### Template 1: Extending Right by One and Shrinking Left Until Valid

The best template I've found so far:

```cpp
int i = 0, j = 0, ans = 0;
for (; j < N; ++j) {
    // CODE: use A[j] to update state which might make the window invalid
    for (; !valid(); ++i) { // when invalid, keep shrinking the left edge until it's valid again
        // CODE: update state using A[i]
    }
    ans = max(ans, j - i + 1); // the window [i, j] is the maximum window we've found thus far
}
return ans;
```

Essentially, we want to **keep the window valid** at the end of each outer `for` loop.

### Template 2: Non-shrinkable Window

```cpp
int i = 0, j = 0;
for (; j < N; ++j) {
    // CODE: use A[j] to update state which might make the window invalid
    if (!valid()) { // Increment the left edge ONLY when the window is invalid. In this way, the window GROWs when it's valid, and SHIFTs when it's invalid
        // CODE: update state using A[i]
        ++i;
    }
    // after `++j` in the for loop, this window `[i, j)` of length `j - i` MIGHT be valid.
}
return j - i; // There must be a maximum window of size `j - i`.
```

Essentially, we GROW the window when it's valid, and SHIFT the window when it's invalid.

## Find Minimum Window

```cpp
int i = 0;
for (int j = 0; j < N; ++j) {
    // use A[j] to update state.
    while (valid()) {
        ans = min(ans, j - i + 1);
        sum -= A[i++];
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
* [1493. Longest Subarray of 1's After Deleting One Element \(Medium\)](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)
* [713. Subarray Product Less Than K (Medium)](https://leetcode.com/problems/subarray-product-less-than-k/)
* [1838. Frequency of the Most Frequent Element (Medium)](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)

Find Minimum and Maximum

* [992. Subarrays with K Different Integers (Hard)](https://leetcode.com/problems/subarrays-with-k-different-integers/)

Fixed Window Size

* [1461. Check If a String Contains All Binary Codes of Size K \(Medium\)](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)
