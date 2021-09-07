# Quick Sort

Core algorithm
* `M = partition(L, R)`
* `quickSort(L, M - 1)`
* `quickSort(M + 1, R)`

## Why do we need to randomly select a pivot?

The performance of quick sort is highly dependent on the selection of the pivot element.

Imagine we want to sort `A = [5,2,3,1,4]`.

If we always choose a bad pivot element, we can only reduce the scale by one each time. The time complexity degrades to `O(N^2)`.

```
pivot = 1, A = [(1),5,2,3,4]
pivot = 2, A = [(1,2),5,3,4]
pivot = 3, A = [(1,2,3),5,4]
pivot = 4, A = [(1,2,3,4,5)]
```

If we always choose a good pivot element, we reduce the scale exponentially. The time complexity is `O(NlogN)`.

```
pivot = 2, A = [2,1,(3),5,4]
pivot = 1, A = [(1,2,3),5,4]
pivot = 4, A = [(1,2,3,4,5)]
```

As you can see, Quick Select performs better if we can approximately partition around the median.

## Algorithm

**Lomuto's partition**:
* Easy to implement
* Less efficient than Hoare's partition.

The logic is very similar to [283. Move Zeroes (Easy)](https://leetcode.com/problems/move-zeroes/).

```cpp
// OJ: https://leetcode.com/problems/sort-an-array/
// Author: github.com/lzl124631x
// Time: O(NlogN) on average, O(N^2) in the worst case
// Space: O(logN) on average, O(N) in the worst case
class Solution {
    int partition(vector<int> &A, int L, int R, int pivot) {
        swap(A[pivot], A[R]);
        for (int i = L; i < R; ++i) { // `i` is the read pointer, `L` is the write pointer
            if (A[i] >= A[R]) continue; // we are looking for numbers < A[R]. So skipping those `>= A[R]`.
            swap(A[i], A[L++]); // once found, swap it to `A[L]` and move `L` forward.
        }
        swap(A[L], A[R]); // swap the pivot value to `A[L]`.
        return L;
    }
    void quickSort(vector<int> &A, int L, int R) {
        if (L >= R) return;
        int M = partition(A, L, R, rand() % (R - L + 1) + L);
        quickSort(A, L, M - 1);
        quickSort(A, M + 1, R);
    }
public:
    vector<int> sortArray(vector<int>& A) {
        srand(NULL);
        quickSort(A, 0, A.size() - 1);
        return A;
    }
};
```

Or

**Hoare's partition**:
* Tricker to implement
* More efficient than Lomuto's partition because it does **three** times fewer swaps on average, and it creates efficient partitions even when all values are equal.

```cpp
// OJ: https://leetcode.com/problems/sort-an-array/
// Author: github.com/lzl124631x
// Time: O(NlogN) on average, O(N^2) in the worst case
// Space: O(logN) on average, O(N) in the worst case
class Solution {
private:
    int partition(vector<int> &A, int L, int R, int pivot) {
        swap(A[pivot], A[R]);
        int i = L, j = R;
        while (i < j) {
            while (i < j && A[i] < A[R]) ++i;
            while (i < j && A[j] >= A[R]) --j;
            swap(A[i], A[j]);
        }
        swap(A[i], A[R]);
        return i;
    }
    void quickSort(vector<int> &A, int L, int R) {
        if (L >= R) return;
        int M = partition(A, L, R, rand() % (R - L + 1) + L);
        quickSort(A, L, M - 1);
        quickSort(A, M + 1, R);
    }
public:
    vector<int> sortArray(vector<int>& A) {
        srand (time(NULL));
        quickSort(A, 0, A.size() - 1);
        return A;
    }
};
```