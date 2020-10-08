# Bit Manipulation

## Bitwise NOT

`~val`

```text
NOT 0111 (decimal 7)
= 1000 (decimal 8)
```

`~val` is also equivalent to `val != -1`, since `-1` is the only one number whose bitwise NOT is `0`.

So if `val` is some non-negative number, the following:

```text
for (int i = val; ~i; --i)
```

is equivalent to

```text
for (int i = val; i >= 0; --i)
```

## Set a bit at `index`

`target |= 1 << index`

## Unset a bit at `index`

`target &= ~(1 << index)`

## Toggle a bit at `index`

`target ^= 1 << index`

## Check a bit at `index`

`(target >> index) & 1`

## Set a bit at `index` with `x`

`target = (target & ~(1 << index)) | (x << index)`

## Get the lowest bit

```cpp
function lowbit(int x) {
    return x & (-x);
}
```

## Count bit 1s

```cpp
__builtin_popcount(n);
```

## Reference

* [https://oi-wiki.org/math/bit/](https://oi-wiki.org/math/bit/)

