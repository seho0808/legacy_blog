---
title: Combinatorics
parent: Algorithms
has_child: false
---
## Pascal's Triangle
#### Equation with binomial coefficients

$$\binom{n}{k} = \binom{n-1}{k-1}\binom{n-1}{k}$$
#### Proof (combinatorial version)

Imagine that you have to choose $k$ balls from $n$ total balls.
Let's say we fix a single ball. We name that ball A.
We can divide $\binom{n}{k}$ into two parts: case where we include A and where we do not include A.
When we do include A, we already chose one ball, so the possible cases equal $\binom{n-1}{k-1}$.
When we do not include A, we have to choose $k$ number of balls from $n-1$ total balls which is $\binom{n-1}{k-1}$. QED.

#### Visualization

![alt text](https://t1.daumcdn.net/cfile/tistory/99F6A64C5A7F39B119)
