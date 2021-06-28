## Heap Sort


## Algorithm

```cpp
// OJ: https://leetcode.com/problems/sort-an-array/
// Author: github.com/lzl124631x
// Time: O(NlogN)
// Space: O(1)
class Solution {
    void heapify(vector<int> &A) {
        for (int i = A.size() / 2 - 1; i >= 0; --i) { // The parent node with the greatest index is `A[N / 2 - 1]`
            siftDown(A, i, A.size()); // Keep sifting down all the parent nodes from bottom up
        }
    }
    void siftDown(vector<int> &A, int hole, int end) {
        int next = 2 * hole + 1; // `next` is the left child of `hole`
        while (next < end) {
            if (next + 1 < end && A[next + 1] > A[next]) ++next; // if the right child is greater than left child, use right child instead.
            if (A[hole] > A[next]) break; // if the parent node is already greater than its greatest child, we don't need to sift it down.
            swap(A[hole], A[next]); // otherwise, keep sifting it down until the parent node `hole` doesn't have any child, i.e. `next >= end`
            hole = next;
            next = 2 * hole + 1;
        }
    }
public:
    vector<int> sortArray(vector<int>& A) {
        heapify(A); // max heap. Heap top at `A[0]`
        for (int i = A.size() - 1; i > 0; --i) {
            swap(A[0], A[i]); // keep swapping heap top to the tail of the heap
            siftDown(A, 0, i); // For the new `A[0]`, sift it down.
        }
        return A;
    }
};
```