# Quick Select

Quickselect is a selection algorithm to find the k-th smallest/largest element in an unordered list. It uses the `partition` method in Quick Sort. The difference is, instead of recurring for both sides \(after finding pivot\), it recurs only for the part that contains the k-th smallest/largest element.

The time complexity is `O(N)` on average, and `O(N^2)` in the worst case.

## Implementation

Quick select with elements sorted in ascending order.

```cpp
// OJ: https://leetcode.com/problems/kth-largest-element-in-an-array/
// Author: github.com/lzl124631x
// Time: O(N) on averge, O(N^2) in the worst case
// Space: O(1)
class Solution {
    int partition(vector<int> &A, int L, int R) {
        int i = L, pivotIndex = L + rand() % (R - L + 1), pivot = A[pivotIndex];
        swap(A[pivotIndex], A[R]);
        for (int j = L; j < R; ++j) {
            if (A[j] < pivot) swap(A[i++], A[j]);
        }
        swap(A[i], A[R]);
        return i;
    }
public:
    int findKthLargest(vector<int>& A, int k) {
        int L = 0, R = A.size() - 1;
        k = A.size() - k + 1;
        while (true) {
            int M = partition(A, L, R);
            if (M + 1 == k) return A[M];
            if (M + 1 > k) R = M - 1;
            else L = M + 1;
        }
    }
};
```

Quick select with elements sorted in descending order.

```cpp
// OJ: https://leetcode.com/problems/kth-largest-element-in-an-array/
// Author: github.com/lzl124631x
// Time: O(N) on averge, O(N^2) in the worst case
// Space: O(1)
class Solution {
    int partition(vector<int> &A, int L, int R) {
        int i = L, pivotIndex = L + rand() % (R - L + 1), pivot = A[pivotIndex];
        swap(A[pivotIndex], A[R]);
        for (int j = L; j < R; ++j) {
            if (A[j] > pivot) swap(A[i++], A[j]);
        }
        swap(A[i], A[R]);
        return i;
    }
public:
    int findKthLargest(vector<int>& A, int k) {
        int L = 0, R = A.size() - 1;
        while (true) {
            int M = partition(A, L, R);
            if (M + 1 == k) return A[M];
            if (M + 1 > k) R = M - 1;
            else L = M + 1;
        }
    }
};
```

Or STL

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

## Reference

* [Quickselect Algorithm - GeeksforGeeks](https://www.geeksforgeeks.org/quickselect-algorithm/)

## Problems

* [215. Kth Largest Element in an Array \(Medium\)](https://leetcode.com/problems/kth-largest-element-in-an-array/)
* [973. K Closest Points to Origin \(Medium\)](https://leetcode.com/problems/k-closest-points-to-origin/)
* [1471. The k Strongest Values in an Array (Medium)](https://leetcode.com/problems/the-k-strongest-values-in-an-array/)
