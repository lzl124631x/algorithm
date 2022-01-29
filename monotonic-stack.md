# Monotonic Stack

A monotonic stack is a stack whose elements are monotonically increasing or descreasing.

Sometimes we store the index of the elements in the stack and make sure the elements corresponding to those indexes in the stack forms a mono-sequence.

## Increasing or decreasing?

If we need to pop **smaller** elements from the stack before pushing a new element, the stack is **decreasing** from bottom to top.

Otherwise, it's **increasing** from bottom to top.

For example,

```
Mono-decreasing Stack

Before: [5,4,2,1]
To push 3, we need to pop smaller (or equal) elements first
After: [5,4,3]
```

## Notes

For a mono-**decreasing** stack:
* we need to pop **smaller** elements before pushing.
* it keep tightening the result as lexigraphically **greater** as possible. (Because we keep popping smaller elements out and keep greater elements).

![](.gitbook/assets/monostack.png)

Take [402. Remove K Digits (Medium)](https://leetcode.com/problems/remove-k-digits/) for example, since we are looking for lexigraphically **smallest** subsequence, we should use mono-**increasing** stack.

## Both nextSmaller and prevSmaller

If we need to calculate both `nextSmaller` and `prevSmaller` arrays, we can do it using a mono-stack in one pass.

The following code is for [84. Largest Rectangle in Histogram (Hard)](https://leetcode.com/problems/largest-rectangle-in-histogram/)

```cpp
// OJ: https://leetcode.com/problems/largest-rectangle-in-histogram/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(N)
class Solution {
public:
    int largestRectangleArea(vector<int>& A) {
        A.push_back(0); // append a zero at the end so that we can pop all elements from the stack and calculate the corresponding areas
        int N = A.size(), ans = 0;
        stack<int> s; // strictly-increasing mono-stack
        for (int i = 0; i < N; ++i) {
            while (s.size() && A[i] <= A[s.top()]) { // Take `A[i]` as the right edge
                int height = A[s.top()]; // Take the popped element as the height
                s.pop();
                int left = s.size() ? s.top() : -1; // Take the element left on the stack as the left edge
                ans = max(ans, (i - left - 1) * height);
            }
            s.push(i);
        }
        return ans;
    }
};
```

## Problems

* [402. Remove K Digits (Medium)](https://leetcode.com/problems/remove-k-digits/)
* [496. Next Greater Element I \(Easy\)](https://leetcode.com/problems/next-greater-element-i/)
* [1019. Next Greater Node In Linked List \(Medium\)](https://leetcode.com/problems/next-greater-node-in-linked-list/)
* [503. Next Greater Element II \(Medium\)](https://leetcode.com/problems/next-greater-element-ii/)
* [1475. Final Prices With a Special Discount in a Shop \(Easy\)](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/)
* [84. Largest Rectangle in Histogram \(Hard\)](https://leetcode.com/problems/largest-rectangle-in-histogram/)
* [85. Maximal Rectangle \(Hard\)](https://leetcode.com/problems/maximal-rectangle/)
* [456. 132 Pattern \(Medium\)](https://leetcode.com/problems/132-pattern/)
* [1504. Count Submatrices With All Ones \(Medium\)](https://leetcode.com/problems/count-submatrices-with-all-ones/)
* [1673. Find the Most Competitive Subsequence (Medium)](https://leetcode.com/problems/find-the-most-competitive-subsequence/)
* [907. Sum of Subarray Minimums (Medium)](https://leetcode.com/problems/sum-of-subarray-minimums/)
* [1856. Maximum Subarray Min-Product (Medium)](https://leetcode.com/problems/maximum-subarray-min-product/)
* [1124. Longest Well-Performing Interval (Medium)](https://leetcode.com/problems/longest-well-performing-interval/)