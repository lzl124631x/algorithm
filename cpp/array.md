# Array

Using array `int[]` is more performant than `vector<int>`. 

The following trick can initialize the array with customized values.

```
int A[10] = { [0] = 0, [1 ... 9] = INT_MAX };
```

See: https://www.geeksforgeeks.org/designated-initializers-c/