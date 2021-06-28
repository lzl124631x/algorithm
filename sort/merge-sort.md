# Merge Sort

## Algorithm

```cpp
// OJ: https://leetcode.com/problems/sort-an-array/
// Author: github.com/lzl124631x
// Time: O(NlogN)
// Space: O(N)
class Solution {
    vector<int> tmp;
    void mergeSort(vector<int> &A, int begin, int end) {
        if (begin + 1 >= end) return;
        int mid = (begin + end) / 2, i = begin, j = mid, k = begin;
        mergeSort(A, begin, mid);
        mergeSort(A, mid, end);
        while (i < mid || j < end) {
            if (j >= end || (i < mid && A[i] < A[j])) tmp[k++] = A[i++];
            else tmp[k++] = A[j++];
        }
        for (i = begin; i < end; ++i) A[i] = tmp[i];
    }
public:
    vector<int> sortArray(vector<int>& A) {
        tmp.assign(A.size(), 0);
        mergeSort(A, 0, A.size());
        return A;
    }
};
```