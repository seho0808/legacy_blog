---
title: GCD & LCM
parent: Algorithms
has_children: false
---
## GCD (최대공약수)
#### Euclidean Algorithm Implementation
유클리드 호제법(Euclidean Algorithm)을 이용. $GCD(a, b) = GCD(b, r)$ where $bx+r = a$ for some $x$. 
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
#### Euclidean Algorithm Proof
#### Euclidean Algorithm Time Complexity Proof
<ins>Part 1</ins>

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

The last equation is due to definition of fibonacci sequence. \square

<ins>Part 2</ins>

Binet Formula states

$$f_N =  \frac{(1+\sqrt{5})^N - (1-\sqrt{5})^N}{2^N\sqrt{5}} \wedge f_N ≈ ∅^N $$$

$$N ≈ \log_{∅}{f_N}$$

where ∅≈1.618. We can see from Part 1, that

$$a \geq f_{N+2} \wedge b \geq f_{N+1}$$

$$\rightarrow f_{N+1} ≈ min(a,b)$$

$$\rightarrow N+1 ≈ \log_{∅}{min(a,b)}$$

$$\therefore O(N) = O(N+1) = log(min(a,b))\square$$

[Original Source GeeksForGeeks Proof](https://www.geeksforgeeks.org/time-complexity-of-euclidean-algorithm/) 

[O(max(loga, logb)) Proof](https://dandalf.tistory.com/123)






## LCM (최소공배수)
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

