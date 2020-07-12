# Binary Index Tree

aka Fenwick Tree.

## Motivation

Fenwick tree was proposed to solve the [mutable range sum query problem](https://leetcode.com/problems/range-sum-query-mutable/).

Query: `O(logN)`

Update: `O(logN)`

## Implementation

```cpp
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

The implementation for [307. Range Sum Query - Mutable \(Medium\)](https://leetcode.com/problems/range-sum-query-mutable/submissions/)

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

## Reference

* [https://leetcode.com/problems/range-sum-query-mutable/discuss/75753/Java-using-Binary-Indexed-Tree-with-clear-explanation](https://leetcode.com/problems/range-sum-query-mutable/discuss/75753/Java-using-Binary-Indexed-Tree-with-clear-explanation)
* [https://www.topcoder.com/community/competitive-programming/tutorials/binary-indexed-trees/](https://www.topcoder.com/community/competitive-programming/tutorials/binary-indexed-trees/)
* [https://oi-wiki.org/ds/fenwick/](https://oi-wiki.org/ds/fenwick/)
* [https://www.youtube.com/watch?v=WbafSgetDDk](https://www.youtube.com/watch?v=WbafSgetDDk)
* [https://visualgo.net/en/fenwicktree](https://visualgo.net/en/fenwicktree)
* [https://www.luogu.com.cn/problem/solution/P3374](https://www.luogu.com.cn/problem/solution/P3374)

