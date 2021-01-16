# Quick Select

Quickselect is a selection algorithm to find the k-th smallest/largest element in an unordered list. It uses the `partition` method in Quick Sort. The difference is, instead of recurring for both sides \(after finding pivot\), it recurs only for the part that contains the k-th smallest/largest element.

The time complexity is `O(N)` on average, and `O(N^2)` in the worst case.

## Implementation

Quick select with elements sorted in ascending order.

**Initialization**:

Randomly select a index `r` and use `A[r]` as the pivot

Swap `A[r]` with the last element `A[R]`.

**Loop and Swap**:
1. Since we swapped pivot to the back, we must move the left pointer `i` first.
2. Then move the right pointer `j`
3. If `i < j`, we swap `A[i]` and `A[j]`.
4. Continue the loop until `i == j`.

**Finally**:

In the end `i == j`, and `A[i] >= pivot`. So we swap `A[i]` with `A[R]`.

**Note**:

* Since we moved pivot to the back, we must move the left pointer `i` first to ensure that after the loop, `A[i] >= pivot` so that we can safely `swap(A[i], A[R])`.
* All the conditions must be `i < j`. In the end `i == j`.
* When comparing `A[i]` or `A[j]` with pivot, must include the equal sign. Numbers that equal `pivot` might scatter in both the left and right part and this is fine.
* For `swap(A[i], A[j])`, we must not do `swap(A[i++], A[j--])`, because that might cause `i == j` and we'll miss checking the last `A[i]`.

```cpp
// OJ: https://leetcode.com/problems/kth-largest-element-in-an-array/
// Author: github.com/lzl124631x
// Time: O(N) on averge, O(N^2) in the worst case
// Space: O(1)
class Solution {
    int quickSelect(vector<int> &A, int L, int R, int r) {
        int pivot = A[r], i = L, j = R;
        swap(A[r], A[R]);
        while (i < j) {
            while (i < j && A[i] <= pivot) ++i;
            while (i < j && A[j] >= pivot) --j;
            if (i < j) swap(A[i], A[j]);
        }
        swap(A[i], A[R]);
        return i;
    }
public:
    int findKthLargest(vector<int>& A, int k) {
        int L = 0, R = A.size() - 1;
        k = A.size() - k;
        srand(NULL);
        while (true) {
            int M = quickSelect(A, L, R, L + rand() % (R - L + 1));
            if (M == k) return A[k];
            if (M > k) R = M - 1;
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
    int quickSelect(vector<int> &A, int L, int R, int r) {
        int pivot = A[r], i = L, j = R;
        swap(A[r], A[R]);
        while (i < j) {
            while (i < j && A[i] >= pivot) ++i; // for descending order, the only two changes we need to make are the comparison signs with the pivot
            while (i < j && A[j] <= pivot) --j;
            if (i < j) swap(A[i], A[j]);
        }
        swap(A[i], A[R]);
        return i;
    }
public:
    int findKthLargest(vector<int>& A, int k) {
        int L = 0, R = A.size() - 1;
        --k;
        while (true) {
            int M = quickSelect(A, L, R, L + rand() % (R - L + 1));
            if (M == k) return A[M];
            if (M < k) L = M + 1;
            else R = M - 1;
        }
    }
};
```

Or use STL

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

