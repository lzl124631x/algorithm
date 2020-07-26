# Why programmers like to set Infinity to 0x3f3f3f3f?

There is no fixed way to represent Infinity. The first option that might come to our mind is `0x7f ff ff ff = 2^31 - 1 = 2,147,483,647` which is the maximum value of 32-bit `int`.

This choice has one issue -- when adding Infinity with another number, you will get overflow error.

A better choice is `0x3f 3f 3f 3f = 1,061,109,567`.

* It is of the same order of magnitude as `0x7f ff ff ff` but twice of it is still within the range of `int`.
* When using `memset` to initialize an array of `int`, since `memset` is filling values as `unsigned char`, `memset(A, 0x3f, sizeof(A))` will perfectly fill `0x3f 3f 3f 3f` into the array.

## Reference

* https://www.kawabangga.com/posts/1153