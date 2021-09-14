# Sieve of Eratosthenes (埃拉托斯特尼筛法)

The sieve of Eratosthenes is an algorithm for finding all prime numbers up to any given limit.

## Algorithm

Start from the first prime `2`, mark all the multiples of `2` as composite (i.e. not prime). So we mark `2*2, 2*3, 2*4, ...` until we reach the greater number in consideration.

Then the next first number after `2` that is not marked as composite is a prime. It's `3`. Then we do the same thing, marking all the multiples of `3` as composite, `3*2, 3*3, 3*4, ...`. Note that since `2*3` has already been marked, so we shouldn't mark it again. Thus, instead of start marking from `2 * 3`, we should start from `3 * 3`, i.e. `sqrt(3)`.

Then the next prime is `5`, we mark all its multiples as composite. Again, we should start marking from `sqrt(5)` since `2*5, 3*5, 4*5` have all been marked already.

And so on. Until we've visited all the numbers within the maximum number.

![](../.gitbook/assets/Sieve_of_Eratosthenes_animation.gif)

## Implementation

```cpp
// OJ: https://leetcode.com/problems/count-primes/
// Author: github.com/lzl124631x
// Time: O(NloglogN)
// Space: O(N)
// Ref: https://leetcode.com/problems/count-primes/solution/
class Solution {
public:
    int countPrimes(int n) {
        if (n <= 1) return 0;
        vector<bool> prime(n, true);
        int ans = 0, bound = sqrt(n);
        for (int i = 2; i < n; ++i) {
            if (!prime[i]) continue;
            ++ans;
            if (i > bound) continue; // If i > bound, then i * i must be greater than `n`, skip. This can prevent overflow caused by `i * i`.
            for (int j = i * i; j < n; j += i) prime[j] = false; // note that we start from `i * i` instead of `2` because all multiples of `2, 3, ..., (i-1)` must be marked already
        }
        return ans;
    }
};
```

## Problems

* [204. Count Primes (Easy)](https://leetcode.com/problems/count-primes/)
* [1175. Prime Arrangements (Easy)](https://leetcode.com/problems/prime-arrangements/)
* [1627. Graph Connectivity With Threshold (Hard)](https://leetcode.com/problems/graph-connectivity-with-threshold/)

## Reference

* https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes