# Quick Sort

Core algorithm
* `M = partition(L, R)`
* `quickSort(L, M - 1)`
* `quickSort(M + 1, R)`

## Algorithm

```cpp
// OJ: https://leetcode.com/problems/sort-an-array/
// Author: github.com/lzl124631x
// Time: O(NlogN) on average, O(N^2) in the worst case
// Space: O(logN) on average, O(N) in the worst case
class Solution {
    int partition(vector<int> &A, int L, int R, int pivot) {
        swap(A[pivot], A[R]);
        for (int i = L; i < R; ++i) {
            if (A[i] >= A[R]) continue;
            swap(A[i], A[L++]);
        }
        swap(A[L], A[R]);
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