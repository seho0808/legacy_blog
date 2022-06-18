---
title: Generating a Random Integer Inside a Range in C++
parent: Algorithm Utility
has_children: false
---
## Generating an integer within the range of [start, end] can be very simple.
```cpp
rand(); // yields a number from 0~32767
rand()%5; // yields a number from 1~4
rand()%10; // yields a number from 1~9
rand()%100-50; // yields a number from -50~50
1000-rand()%100; // yields a number from 901~1000
10-rand()%5; // yields a number from 6~10
10-rand()%6; // yields a number from 5~10
```
## Here is a function
```cpp
int rand_range(int start, int end) {
  return end-rand()%(start+1);
}
```
