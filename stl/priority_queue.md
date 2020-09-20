# Priority Queue

By default:

* `priority_queue` is a **Max-heap**. The element with the highest priority is at the top.
* `priority_queue` uses `less` as comparator. \(In fact, STL always use `less` as the default comparator whenever it needs comparison.\)

If you want to get a Min-heap, use `priority_queue<int, vector<int>, greater<int>>`.

It uses `make_heap`, `push_heap` and `pop_heap` algorithm functions under the hood.

## Why is it Max-heap by default?

> The elements are compared using operator`<` \(by default\), or `comp` \(custom comparator\): The element with the highest value is an element for which this would return `false` when compared to every other element in the range.

[http://www.cplusplus.com/reference/algorithm/make\_heap](http://www.cplusplus.com/reference/algorithm/make_heap)

In priority queue, the element with the highest priority is the one which gets `false` when comparing with every other element using the comparator. The comparator is by default `less`. So the element that is not less than any other elements, i.e. the largest one, is at the top.

```text
           front                           back 
             v                               v
vector: [   10,              ...             2  ]
             ^
          heap top
```

After poping the top

```text
           front                        back     popped element
             v                           v              v
vector: [    8,              ...         2  ]        [ 10 ]
             ^
          heap top
```

## Related functions

Note: try to solve [215. Kth Largest Element in an Array \(Medium\)](https://leetcode.com/problems/kth-largest-element-in-an-array/) using the following functions.

### `make_heap`

Rearranges the elements in the range `[first,last)` in such a way that they form a _heap_.

The element with the highest value is always pointed by _first_.

```cpp
// range heap example
#include <iostream>     // std::cout
#include <algorithm>    // std::make_heap, std::pop_heap, std::push_heap, std::sort_heap
#include <vector>       // std::vector

int main () {
    int myints[] = {10,20,30,5,15};
    std::vector<int> v(myints,myints+5);

    std::make_heap (v.begin(),v.end());
    std::cout << "initial max heap   : " << v.front() << '\n';

    std::pop_heap (v.begin(),v.end()); v.pop_back();
    std::cout << "max heap after pop : " << v.front() << '\n';

    v.push_back(99); std::push_heap (v.begin(),v.end());
    std::cout << "max heap after push: " << v.front() << '\n';

    std::sort_heap (v.begin(),v.end());

    std::cout << "final sorted range :";
    for (unsigned i=0; i<v.size(); i++)
        std::cout << ' ' << v[i];

    std::cout << '\n';

    return 0;
}
```

Output:

```text
initial max heap   : 30
max heap after pop : 20
max heap after push: 99
final sorted range : 5 10 15 20 99
```

### `push_heap`

Given a heap in the range `[first,last-1)`, this function extends the range considered a heap to `[first,last)` by placing the value in `(last-1)` into its corresponding location within it.

### `pop_heap`

Rearranges the elements in the heap range `[first,last)` in such a way that the part considered a heap is shortened by one: The element with the highest value is moved to `(last-1)`.

### `sort_heap`

Sorts the elements in the heap range `[first,last)` into ascending order.

The range loses its properties as a heap.

## Custom Comparator

```cpp
// Assume there is a `m` variable in the context and `m[a]` is the frequency of `a`
// In following code, the element with the highest frequency is the root.
auto cmp = [&](int a, int b) { return m[a] < m[b]; };
priority_queue<int, vector<int>, decltype(cmp)> q(cmp);
```

Another way

```cpp
class Cmp {
public:
    bool operator() (const pair<int, int>& a, const pair<int, int>& b) const {
        return a.second < b.second;
    }
};

priority_queue<pair<int, int>, vector<pair<int, int>>, Cmp> q(m.begin(), m.end()); // Assume there is an `m` storing pairs of `value, frequency`.
```