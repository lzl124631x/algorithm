# Modular Multiplicative Inverse

A \[modular multiplicative inverse]\[1] of an integer $a$ is an integer $x$ such that $a\cdot x$ is congruent to 1 modular some modulus $m$. To write it in a formal way: we want to find an integer $x$ so that

$$a\cdot x\equiv1\;(mod\;m)$$

We will also denote $x$ simply with $a^{−1}$.

For example, assume $a = 5, m = 13$, then $a^{-1} = 8$ because $5\cdot 8 \mod 13 = 1$.

We should note that the modular inverse does not always exist. For example, let $m=4, a=2$. By checking all possible values modulo $m$ is should become clear that we cannot find $a^{−1}$ satisfying the above equation. It can be proven that the modular inverse exists if and only if $a$ and $m$ are relatively prime (i.e. $gcd(a,m)=1$).

In this article, we present two methods for finding the modular inverse in case it exists, and one method for finding the modular inverse for all numbers in linear time.

## Finding the Modular Inverse using Extended Euclidean algorithm

Consider the following equation (with unknown $x$ and $y$):

$$a\cdot x+m\cdot y=1$$

This is a [Linear Diophantine equation in two variables](https://cp-algorithms.com/algebra/linear-diophantine-equation.html). As shown in the linked article, when $gcd(a,m)=1$, the equation has a solution which can be found using the extended Euclidean algorithm. Note that $gcd(a,m)=1$ is also the condition for the modular inverse to exist.

> NOTE: [Diophantine equation](https://en.wikipedia.org/wiki/Diophantine\_equation) (丢番图方程, 不定方程)

Now, if we take modulo m of both sides, we can get rid of m⋅y, and the equation becomes:

a⋅x≡1modm Thus, the modular inverse of a is x.

The implementation is as follows:

## Reference

* [https://oi-wiki.org/math/inverse/](https://oi-wiki.org/math/inverse/)

\[1]: [https://en.wikipedia.org/wiki/Modular\_multiplicative\_inverse](https://en.wikipedia.org/wiki/Modular\_multiplicative\_inverse)
