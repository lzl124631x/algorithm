# Mono-deque

One typical usage of mono-deque is to find the maximum/minimum value in a sliding window.

If we are looking for the **maximum** value, we can use a **descending** monoqueue. Example: [239. Sliding Window Maximum (Hard)](https://leetcode.com/problems/sliding-window-maximum/).

## How to memorize?

If we are looking for the **maximum** values in a sliding window. When a large number goes into window, for all the smaller or equal numbers in front of this number, their eviction from the window won't affect the max value of the sliding window, so they can be discarded. In other words, the mono-deque will only contain the numbers that are greater than the current new number, so it's a **desending** mono-deque.

## Problems

* [239. Sliding Window Maximum (Hard)](https://leetcode.com/problems/sliding-window-maximum/)
* [862. Shortest Subarray with Sum at Least K \(Hard\)](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)
* [1499. Max Value of Equation \(Hard\)](https://leetcode.com/problems/max-value-of-equation/)
* [1696. Jump Game VI (Medium)](https://leetcode.com/problems/jump-game-vi/)
