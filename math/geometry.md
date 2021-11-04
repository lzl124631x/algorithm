# Geometry

## Dot Product of Vectors

Dot product is also known as **scalar product** or **inner product**.


Algebraically, the dot product is the sum of the products of the corresponding entries of the two sequences of numbers.

$$
\bm{a}\cdot \bm{b} = \sum_{i=1}^na_ib_i
$$

Geometrically, it is the product of the Euclidean magnitudes of the two vectors and the cosine of the angle between them.

$$
\bm{a}\cdot \bm{b}=\lvert \bm{a}\rvert\lvert \bm{b}\rvert\cos\theta
$$

### Usage

1. Calculate the angle between two vectors.

$$
\cos\theta
=\frac{\bm{a}\cdot \bm{b}}{\lvert \bm{a}\rvert\lvert \bm{b}\rvert}$$

2. If two vectors are perpendicular, the dot product is `0`.

### Problem

* [963. Minimum Area Rectangle II (Medium)](https://leetcode.com/problems/minimum-area-rectangle-ii/)

## Cross Product of Vectors

Cross product is also known as **vector product** ((occasionally **directed area product**, to emphasize its geometric significance) )

The cross product $\bm{a}\times\bm{b}$ is defined as $\bm{a}$ vector $\bm{c}$ that is perpendicular (orthogonal) to both $\bm{a}$ and $\bm{b}$, with $\bm{a}$ direction given by the right-hand rule and a magnitude equal to the area of the parallelogram that the vectors span.

$$
\bm{a}\times\bm{b}=\lvert\bm{a}\rvert\lvert\bm{b}\rvert\sin(\theta)\bm{n}
$$

where $\bm{n}$ is a unit vector perpendicular to the plane containing $\bm{a}$ and $\bm{b}$.

![Finding the direction of the cross product by the right-hand rule.](../.gitbook/assets/cross-product-right-hand-rule.png)