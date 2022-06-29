---
title: Number Theory
parent: Algorithms
has_child: false
---
## Table of Contents
- [Prime Factor of Factorial](#1)
- [Fermat's Little Theorem](#2)

## Prime Factor of Factorial<a name="1"></a>
#### Equation
Assume $N$ as an integer. Below equation is a factorized version of $N!$.

$$ N! = 2^a\times3^b\times5^c\times7^d\times\cdots $$

Then,

$$ a = N/2 + N/4 + N/8 + N/16 + \cdots $$

$$ b = N/3 + N/9 + N/27 + N/81 + \cdots $$

$$ \cdots $$


#### Explanation
Let's take 5 for example. For step 1, every number with a single 5 as factor is counted. Then, for step 2,
every number with two 5s as factor is counted. However, since 
the numbers in the second step are all included in the first step,we only have to add 1 for each count of step2.
For step 3, all numbers that has three 5s as factors were already counted twice in step 1 and 2, so we only
have to add 1 for each count of step 3 as well. This applies upto the nth step.

## Fermat's Little Theorem <a name="2"></a>
