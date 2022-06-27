---
title: GCD & LCM
parent: Algorithms
has_children: false
---
## GCD (최대공약수)
유클리드 호제법을 이용. $GCD(a, b) = GCD(b, r)$ where $bx+r = a$ for some $x$. 
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

## LCM (최소공배수)
$$LCM(a, b) = \frac{a \times b}{GCD(a, b)}$$
```cpp
int lcm(int a, int b) {
    return a*(b/gcd(a, b)); // overflow 최적화
}
```
