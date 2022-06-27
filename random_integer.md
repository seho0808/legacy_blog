---
title: Random Integer in Range C++
parent: Algorithms
has_children: false
---
## Generating an integer within a range
```cpp
rand(); // yields a number from 0~32767
rand()%5; // yields a number from 0~4
rand()%10; // yields a number from 0~9
rand()%100-50; // yields a number from -50~50
1000-rand()%100; // yields a number from 901~1000
10-rand()%5; // yields a number from 6~10
10-rand()%6; // yields a number from 5~10
```
## Here is a function
```cpp
// generates an integer from start to end.
// start and end cannot be negative integers.
int rand_range(int start, int end) {
  return start+rand()%(end-start+1);
}
```
