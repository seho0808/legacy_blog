---
title: GCD & LCM
parent: Algorithms
has_children: false
---
## Euclidean Algorithm
#### Equation
$GCD(a, b) = GCD(b, r)$ where $bx+r = a$ for some $x$.
#### Implementation
If we iterate Euclidean Algorithm until $b$ is zero, we get $gcd(a, b)$ in $O(\log_2{min(a,b)})$.
```cpp
int gcd(int a, int b){
	while(b!=0){
		int r = a%b;
		a = b;
		b = r;
	}
	return a;
}
```
## Euclidean Algorithm Proof
## Euclidean Algorithm Time Complexity Proof
#### Summary & Intuition

You can express a single loop of Euclidean Algorithm as,

$$ a = floor(a, b)*b + a\%b \quad (1)$$

Since $a \geq b$ was a premise for $gcd$, $floor(a/b)\geq1$.

If we look at the equation (1), we realize that $a$ and $b$ are
the first pair to be processed in the iteration, and $b$ and $a\%b$ are
the second pair to be processed. If we call $a$ as $k_{n}$ and $b$ as $k_{n-1}$ and
$a\%b$ as $k_{n-2}$ where $n$ is assumed to be the iteration count required to
make $gcd(a, b)$ zero, we can see a pattern that looks like a fibonacci sequence. We
basically add two terms to get the next one. However, since $floor(a/b)\geq1$,
we see that 

$$ a \geq b + a\%b \quad \because floor(a/b)\geq1 $$

$$ \rightarrow k_n \geq k_{n-1} + k_{n-2}$$

Hence, the speed of decrement of k's are at least the speed of fibonacci sequence in
reverse order. Since a number in fibonacci sequence is obtained by some exponential function
from binet equation, it takes log order of iteration to make $gcd(a, b)$ zero.
However, I still haven't figured out why someone would take the minimum of $a$ and $b$ instead of
just $b$ in the equation $O(\log{min(a,b)})$ since $a \geq b$ is the premise, and which means
$\log{min(a,b)} = \log{b}$.


#### Part 1

Statement:

If $gcd(a, b)$ can be reduced to $gcd(x, 0)$ in $N$steps for some x,
$a \geq f_{N+2}$ and $b \geq f_{N+1}$. $f_{N}$ is the $N$th fibonacci number.

Base Case:

Assume $a=2$, $b=1$. $gcd(2,1)$ becomes $gcd(1,0)$ in one step. Hence, $N=1$. 

$$ 2 \geq f_{1+2} \rightarrow a \geq f_{N+2} $$

$$ 1 \geq f_{1+1} \rightarrow b \geq f_{N+1} $$

Inductive Step:

Assume that the statement holds true for $(N-1)$th step. Now we prove the statement for $N$th step is true.
Since the statement for $(N-1)$th step is true, $gcd(b, a%b)$ can be reduced to $gcd(x, 0)$ in $N-1$ steps. Hence,

$$b \geq f_{N-1+2} \rightarrow b \geq f_{N+1}$$

$$a\%b \geq f_{N-1+1} \rightarrow a\%b \geq f_{N}$$

Since $a\%b$ is a remainder of $a/b$,

$$a=floor(a/b)*b + a\%b$$

From above result and that $a \geq b$,

$$a \geq b + (a\%b)$$

$$\rightarrow a \geq f_{N+1} + f_N$$

$$\rightarrow a \geq f_{N+2}$$

The last equation is due to definition of fibonacci sequence. $\square$

#### Part 2

Binet Formula states

$$f_N =  \frac{(1+\sqrt{5})^N - (1-\sqrt{5})^N}{2^N\sqrt{5}} \rightarrow f_N ≈ ∅^N $$

$$\rightarrow N ≈ \log_{∅}{f_N}$$

where ∅≈1.618. We can see from Part 1, that

$$a \geq f_{N+2} \wedge b \geq f_{N+1}$$

$$\rightarrow f_{N+1} ≈ min(a,b)$$

$$\rightarrow N+1 ≈ \log_{∅}{min(a,b)}$$

$$\therefore O(N) = O(N+1) = log(min(a,b))\square$$

[Original Source GeeksForGeeks Proof](https://www.geeksforgeeks.org/time-complexity-of-euclidean-algorithm/) 

[O(max(loga, logb)) Proof](https://dandalf.tistory.com/123)






## LCM
#### Equation
$$LCM(a, b) = \frac{a \times b}{GCD(a, b)}$$
#### Implementation
```cpp
int lcm(int a, int b) {
    return a*(b/gcd(a, b)); // overflow optimization
}
```
#### Proof
Let $L$ be $LCM(a, b)$. Let $G$ be $GCD(a, b)$. Then,

$$a = Gx_1$$

$$b = Gx_2$$

$$a*b = G^2x_1x_2$$

$$(a*b)/G = Gx_1x_2 = LCM(a, b)$$

$x_1$ and $x_2$ are coprime by definition of gcd. Hence,
smallest possible multiples of $Gx_1$ and $Gx_2$ to acheive same number 
are $x_2$ and $x_1$ respectively. That is, $LCM(a, b) = Gx_1x_2$ $\square$

