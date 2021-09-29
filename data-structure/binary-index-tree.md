# Binary Index Tree

aka Fenwick Tree.

## Motivation

Fenwick tree was proposed to solve the [mutable range sum query problem](https://leetcode.com/problems/range-sum-query-mutable/).

Query: `O(logN)`

Update: `O(logN)`

## Implementation

```cpp
// Author: github.com/lzl124631x
class BIT {
    vector<int> sum;
    static inline int lowbit(int x) { return x & -x; }
public:
    BIT(int N) : sum(N + 1) {};
    void update(int i, int delta) { // Note: this `i` is 1-based.
        for (; i < sum.size(); i += lowbit(i)) sum[i] += delta;
    }
    int query(int i) { // Note: this `i` is 1-based.
        int ans = 0;
        for (; i; i -= lowbit(i)) ans += sum[i];
        return ans;
    }
    int rangeQuery(int i, int j) { // Note: these `i` and `j` are 1-based.
        return query(j) - query(i - 1);
    }
};
```

The implementation for [307. Range Sum Query - Mutable \(Medium\)](https://leetcode.com/problems/range-sum-query-mutable/)

```cpp
// OJ: https://leetcode.com/problems/range-sum-query-mutable/
// Author: github.com/lzl124631x
// Time: 
//      NumArray: O(N^2 * logN)
//      update: O(logN)
//      sumRange: O(logN)
// Ref: https://www.youtube.com/watch?v=WbafSgetDDk
class BIT {
    vector<int> sum;
    static inline int lowbit(int x) { return x & -x; }
public:
    BIT(int N) : sum(N + 1) {};
    void update(int i, int delta) {
        for (; i < sum.size(); i += lowbit(i)) sum[i] += delta;
    }
    int query(int i) {
        int ans = 0;
        for (; i; i -= lowbit(i)) ans += sum[i];
        return ans;
    }
    int rangeQuery(int i, int j) {
        return query(j) - query(i - 1);
    }
};
class NumArray {
    BIT tree;
    vector<int> nums;
public:
    NumArray(vector<int>& nums) : nums(nums), tree(nums.size()) {
        for (int i = 0; i < nums.size(); ++i) tree.update(i + 1, nums[i]);
    }

    void update(int i, int val) {
        tree.update(i + 1, val - nums[i]);
        nums[i] = val;
    }

    int sumRange(int i, int j) {
        return tree.rangeQuery(i + 1, j + 1);
    }
};
```

The implementation for [1649. Create Sorted Array through Instructions (Hard)](https://leetcode.com/problems/create-sorted-array-through-instructions/)

```cpp
// OJ: https://leetcode.com/problems/create-sorted-array-through-instructions/
// Author: github.com/lzl124631x
// Time: O(NlogM) where M is the range of A[i]
// Space: O(M)
// Ref: https://leetcode.com/problems/create-sorted-array-through-instructions/discuss/927531/JavaC%2B%2BPython-Binary-Indexed-Tree
int c[100001] = {};
class Solution {
public:
    static inline int lowbit(int x) { return x & -x; }
    int createSortedArray(vector<int>& A) {
        memset(c, 0, sizeof(c));
        int ans = 0, N = A.size(), mod = 1e9 + 7;
        for (int i = 0; i < N; ++i) {
            ans = (ans + min(get(A[i] - 1), i - get(A[i]))) % mod;
            update(A[i]);
        }
        return ans;
    }
    void update(int x) {
        for (; x < 100001; x += lowbit(x)) c[x]++;
    }
    int get(int x) { // returns the sum of numbers smaller than x
        int ans = 0;
        for (; x > 0; x -= lowbit(x)) ans += c[x];
        return ans;
    }
};
```

## Note

When querying, keep ADDing low bit.

When updating, keep REMOVING low bit.

## Problems

* [307. Range Sum Query - Mutable \(Medium\)](https://leetcode.com/problems/range-sum-query-mutable/)
* [1649. Create Sorted Array through Instructions (Hard)](https://leetcode.com/problems/create-sorted-array-through-instructions/)

## Reference

* [https://leetcode.com/problems/range-sum-query-mutable/discuss/75753/Java-using-Binary-Indexed-Tree-with-clear-explanation](https://leetcode.com/problems/range-sum-query-mutable/discuss/75753/Java-using-Binary-Indexed-Tree-with-clear-explanation)
* [https://www.topcoder.com/community/competitive-programming/tutorials/binary-indexed-trees/](https://www.topcoder.com/community/competitive-programming/tutorials/binary-indexed-trees/)
* [https://oi-wiki.org/ds/fenwick/](https://oi-wiki.org/ds/fenwick/)
* [https://www.youtube.com/watch?v=WbafSgetDDk](https://www.youtube.com/watch?v=WbafSgetDDk)
* [https://visualgo.net/en/fenwicktree](https://visualgo.net/en/fenwicktree)
* [https://www.luogu.com.cn/problem/solution/P3374](https://www.luogu.com.cn/problem/solution/P3374)
* [树状数组（Binary Indexed Tree），看这一篇就够了](https://blog.csdn.net/Yaokai_AssultMaster/article/details/79492190)