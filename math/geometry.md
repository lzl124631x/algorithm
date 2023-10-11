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

## Check if three points are one te same line

Say we have 3 points, `(x1, y1)`, `(x2, y2)` and `(x3, y3)`. Check if they are on the same line.

Normally we can check if slopes are the same.

$$
\frac{y_2-y_1}{x_2-x_1} = \frac{y_3-y_2}{x_3-x_2}
$$

To avoid:
1. The precision error we might get from the divisions
2. Divide by zero issue

We can use the following equation:

$$
(y_2-y_1)\cdot(x_3-x_2) = (y_3-y_2)\cdot(x_2-x_1)
$$

Example question: [2280. Minimum Lines to Represent a Line Chart (Medium)](https://leetcode.com/problems/minimum-lines-to-represent-a-line-chart)