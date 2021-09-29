# lower_bound and upper_bound

## lower_bound + comparator

```cpp
struct Item {
    int start, end;
    string string() { return "(" + to_string(start) + "," + to_string(end) + ")"; }
};
int main() {
    /*
    <=
    0   F F F F => A[0] = (10,60)        
    20  T T F F => A[2] = (30,35)
    40  T T T T => not found
    50  T T T T => not found

    <
    0   F F F F => A[0] = (10,60)        
    20  T F F F => A[1] = (20,40)
    40  T T T F => A[3] = (40,50)
    50  T T T T => not found

    >=  (Note that we shouldn't use this operator because this comparator doesn't split the array into TRUE then FALSE parts)
    0   T T T T => not found
    20  F T T T => not found
    40  F F F T => A[0] = (10,60)
    50  F F F F => A[0] = (10,60)
    */
    vector<Item> A = {{10,60},{20,40},{30,35},{40,50}};
    for (int target : {0, 20, 40, 50}) {
        int j = lower_bound(begin(A), end(A), target, [](auto &val, int target) {
            cout << "comparing " << val.string() << " and " << target << endl;
            return val.start >= target; // change this operator to see the above results
        }) - begin(A);
        cout << target << " => ";
        if (j == A.size()) {
            cout << "NOT FOUND" << endl;
        } else {
            cout << "A[" << j << "] = " << A[j].string() << endl;
        }
    }
}
```

**Summary**:

* `comparator` is in form `(element, target)`.
* The comparator returns `true` if the first argument (`element`) is ordered **before** the second argument (`target`). It separates the array into `TRUE` segment followed by `FALSE` segment.
* `lower_bound` returns the first element returning `false`.

```
                 ┌-- the returned index
                 |
      true       v         false
[--------------] [----------------------]
```

## upper_bound + comparator

```cpp
struct Item {
    int start, end;
    string string() { return "(" + to_string(start) + "," + to_string(end) + ")"; }
};
int main() {
    /*
    <=
    0   T T T T => A[0] = (10,60)        
    20  F T T T => A[1] = (20,40)
    40  F F F T => A[3] = (40,50)
    50  F F F F => not found

    <
    0   T T T T => A[0] = (10,60)        
    20  F F T T => A[2] = (30,35)
    40  F F F F => not found
    50  F F F F => not found

    >= (Note that we shouldn't use this operator because this comparator doesn't split the array into FALSE then TRUE parts)
    0   F F F F => not found
    20  T T F F => not found
    40  T T T T => A[0] = (10,60)
    50  T T T T => A[0] = (10,60)
    */
    vector<Item> A = {{10,60},{20,40},{30,35},{40,50}};
    for (int target : {0, 20, 40, 50}) {
        int j = upper_bound(begin(A), end(A), target, [](int target, auto &val) {
            cout << "comparing " << val.string() << " and " << target << endl;
            return target >= val.start;
        }) - begin(A);
        cout << target << " => ";
        if (j == A.size()) {
            cout << "NOT FOUND" << endl;
        } else {
            cout << "A[" << j << "] = " << A[j].string() << endl;
        }
    }
}
```
**Summary**

* `comparator` is in form `(target, element)`.
* The comparator returns `true` if the first argument (`target`) is ordered **before** the second argument (`element`). It separates the array into `FALSE` segment followed by `TRUE` segment.
* `upper_bound` returns the first element returning `TRUE`.

```
                 ┌-- the returned index
                 |
      false      v        true 
[--------------] [----------------------]
```

# Problems

* [1751. Maximum Number of Events That Can Be Attended II (Hard)](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended-ii/)