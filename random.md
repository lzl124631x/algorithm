# Random

## Random Integer

```cpp
int randomInt(int mn, int mx) {
    return mn + rand() % (mx - mn);
}
```

## Random Double

```cpp
double randomDouble(double mn, double mx) {
    double d = (double)rand() / RAND_MAX;
    return mn + d * (mx - mn);
}
```