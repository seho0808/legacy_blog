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
#### Euclidean Algorithm Time Complexity Proof
Base Case:
$$a\geq  $$
Inductive Step:



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
are $x_2$ and $x_1$ respectively. That is, $LCM(a, b) = Gx_1x_2$ $\blacksquare$

