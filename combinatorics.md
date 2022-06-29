---
title: Combinatorics
parent: Algorithms
has_child: false
---
## Table of Contents
- [Pascal's Triangle](#1)
- [Binomial Coefficient Calculation](#2)

## Pascal's Triangle <a name="1"></a>
#### Equation with binomial coefficients

$$\binom{n}{k} = \binom{n-1}{k-1}\binom{n-1}{k}$$
#### Proof (combinatorial version)

Imagine that you have to choose $k$ balls from $n$ total balls.
Let's say we fix a single ball. We name that ball A.
We can divide $\binom{n}{k}$ into two parts: case where we include A and where we do not include A.
When we do include A, we already chose one ball, so the possible cases equal $\binom{n-1}{k-1}$.
When we do not include A, we have to choose $k$ number of balls from $n-1$ total balls which is $\binom{n-1}{k-1}$. $\square$

#### Visualization

![alt text](https://t1.daumcdn.net/cfile/tistory/99F6A64C5A7F39B119)

## Binomial Coefficient Calculation <a name="2"></a>
#### With Fermat's Little Theorem (O(logn))
Assume $p$ is some prime number.

$$ nCk \% p = \frac{n!}{k!(n-k)!} \% p$$

Let $A=n! \wedge B=k!(n-k)!$.

$$ nCk \% p = (A*B^{-1}) \% p $$

By distributive property of modulo,

$$ nCk \% p = ((A \% p)* (B^{-1} \% p)) \% p $$

By Fermat's Little Theorem, 

$$ nCk \% p = (A*B^(p-2)) \% p (\because )$$
