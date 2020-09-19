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

## Tip 2

To find maximum, I previously have been using this template:

```cpp
int i = 0, j = 0;
while (j < N) {
    while (j < N && valid()) ++j;
    ans = max(ans, j - i);
    while (!valid()) ++j;
}
```

Now I found this template is simpler:

```cpp
int i = 0, j = 0;
for (; j < N; ++j) {
    // use A[j] to update state.
    while (!valid()) ++i; // and update state using A[i]
    ans = max(ans, j - i);
}
```

Because with this template, you don't need to think about when to stop extending `j`, we just always extending `j`.

Similarly, to find minimum window:

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
* [1493. Longest Subarray of 1's After Deleting One Element \(Medium\)](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/submissions/)

Find Minimum and Maximum

* [992. Subarrays with K Different Integers (Hard)](https://leetcode.com/problems/subarrays-with-k-different-integers/)

Fixed Window Size

* [1461. Check If a String Contains All Binary Codes of Size K \(Medium\)](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)
