---
title: Quick Sort
parent: Algorithms
has_children: false
---
## Quick Sort (퀵소트 / 퀵정렬)

이 글은 퀵정렬의 원리에 대해 설명하지 않는다.  




목차
1. 시간 복잡도
	1. [Average-Case Time Complexity 증명](#ac_proof)
2. [공간 복잡도](#space_complexity)
3. [최적화 완료된 알고리즘](#3)
4. 최적화 순서
	1. [첫 어프로치](#4-1)
	2. [left+1](#4-2)
	3. [while(p>=v[i])i++;](#4-3)
	4. [i < j 추가](#4-4)
	5. [i와 j 순서 바꿔주기](#4-5)
	6. [swap에 있는 i < j 제거](#4-6)
5. 다른 풀이 세 가지
	1. [pivot을 오른쪽으로 두기](#5-1)
	2. [pivot을 가운데에 냅둔 상태에서 재귀](#5-2)
	3. [메모리를 사용하는 퀵소트](#5-3)
## 시간 복잡도
### Average-Case 증명 <a name="ac_proof"></a>
퀵소트 pivot이 오름차순 순서상 백분위 25%~75% 사이의 값을 계속 얻는다고 가정한다.  
그러면 최악의 경우 1/4 3/4 두 개로 하나의 파티션이 쪼개진다.  
```
       1/4        3/4
       /  \       /  \
      3/4 1/4    1/4 3/4
      ...
```
각 level 마다 남는 최대 크기가 3/4이고, 최악의 경우 3/4으로만 쪼개져서 마지막 노드가 1개가 남을 수 있다.  
그럴 경우, 3/4을 height만큼 곱했을 때 n이 1이되는 것이니까, $n(3/4)^h = 1$이 된다.

$$n*(3/4)^h = 1$$

$$(3/4)^h = 1/n$$

$$3^h/4^h = 1/n$$

$$4^h/3^h = n$$

$$(4/3)^h = n$$

$$\log_{4/3}n = h$$

위와 같이 단순화하면 파티션 트리의 height는 $\log_{4/3}n$이 된다. 여기서 이제 25%\~70%가 아닌 값들을 뽑을 확률이랑
나머지 값들을 뽑을 확률이 동일하기 때문에, 각 level 마다 절반이 1과 n-1로 나누어지는 최악의 케이스와 나머지 절반은 3/4와 1/4로 나누어지는
최악의 케이스를 볼 때, 깊이가 딱 기존의 두 배가 되기 때문에, 트리의 깊이는 평균적으로 $2\log_{4/3}n$가 된다.

각 파티션 마다 O(n)번 도는 것이 worst-case이기에, $O(2n\log_{4/3}n)≈O(n\log_2n)$으로 본다. $\blacksquare$

### 참고

- 위의 Average-Case 증명은 [위키피디아](https://en.wikipedia.org/wiki/Quicksort)에서 읽은 증명임. 아래는 원문.


```
Average-case analysis
To sort an array of n distinct elements, quicksort takes O(n log n) time in expectation, averaged over all n! permutations of n elements with equal probability. We list here three common proofs to this claim providing different insights into quicksort's workings.

Using percentiles
If each pivot has rank somewhere in the middle 50 percent, that is, between the 25th percentile and the 75th percentile, then it splits the elements with at least 25% and at most 75% on each side. If we could consistently choose such pivots, we would only have to split the list at most ${\displaystyle \log _{4/3}n}$ times before reaching lists of size 1, yielding an O(n log n) algorithm.

When the input is a random permutation, the pivot has a random rank, and so it is not guaranteed to be in the middle 50 percent. However, when we start from a random permutation, in each recursive call the pivot has a random rank in its list, and so it is in the middle 50 percent about half the time. That is good enough. Imagine that a coin is flipped: heads means that the rank of the pivot is in the middle 50 percent, tail means that it isn't. Now imagine that the coin is flipped over and over until it gets k heads. Although this could take a long time, on average only 2k flips are required, and the chance that the coin won't get k heads after 100k flips is highly improbable (this can be made rigorous using Chernoff bounds). By the same argument, Quicksort's recursion will terminate on average at a call depth of only ${\displaystyle 2\log _{4/3}n}2$. But if its average call depth is O(log n), and each level of the call tree processes at most n elements, the total amount of work done on average is the product, O(n log n). The algorithm does not have to verify that the pivot is in the middle half—if we hit it any constant fraction of the times, that is enough for the desired complexity.
```
- [칸아카데미](https://www.khanacademy.org/computing/computer-science/algorithms/quick-sort/a/analysis-of-quicksort) 설명 좋아보인다. 나중에 참고하여 읽기.

## 공간 복잡도<a name="space_complexity"></a>
각 파티션 마다 모두 하나의 배열을 포인터로 레퍼런싱하기 때문에 하나의 배열의 메모리 O(n)과 
임시저장 메모리인 스택에 쌓이는 재귀함수들의 메모리 O(log(n))을 합쳐서 O(n+log(n)) = O(n)이 된다.
[스택 참고](https://velog.io/@wonhee010/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0-feat.-%EC%9E%AC%EA%B7%80-vs-%EB%B0%98%EB%B3%B5%EB%AC%B8)

## 최적화 완료된 알고리즘<a name="3"></a>
```cpp
#include <vector>
#include <iostream>

using namespace std;

void print_v(vector<int> v) {
    for (int i = 0 ; i< v.size();i++) {
        cout << v[i] << " ";
    }
}

void swap(vector<int> &v, int i, int j) {
    int temp = v[i];
    v[i] = v[j];
    v[j] = temp;
}

int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left;
    int j = right;
    swap(v, pivot_index, left);
    while (i < j) {
        while (pivot < v[j]) {
            j--;
        }
        while (i < j && pivot >= v[i]) {
            i++;
        }
        swap(v, i, j);
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}

void quick_sort(vector<int> &v, int left, int right) {
    if (left > right) {
        return;
    }
    
    int pivot = partition(v, left, right); 
    
    quick_sort(v, left, pivot-1);
    quick_sort(v, pivot+1, right);
}


int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    
    int n;
    cin >> n;
    vector<int> v(n, 0);
    for (int i = 0; i < n; i++) {
        cin >> v[i];
    }
    quick_sort(v, 0, n-1);
    print_v(v);
    
    return 0;
}
```
백준 2750번으로 테스팅 가능하다. 이제 이 알고리즘의 최적화에 대해서 이야기한다.

## 최적화 순서<a name="4"></a>
### 1. 첫 어프로치<a name="4-1"></a>
파티션 부분만 살펴본다. 랜덤으로 하나의 element를 가장 왼쪽과 교환한다.
그리고 왼쪽에서 올라가는 i를 최대한 올려주고 j는 최대한 내려준다.
```cpp
int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left;
    int j = right;
    swap(v, pivot_index, left);
    while (i < j) {
        while (pivot > v[i]) {
            i++;
        }
        while (pivot < v[j]) {
            j--;
        }
        if (i < j) {
            swap(v, i, j);
        }
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}
```
이 코드의 반례는 [5, 4, 3, 2, 1]이다. 첫 루프에서 2를 pivot으로 잡으면
[2, 4, 3, 5, 1]이 된다. 여기서 i가 2에서 멈추고 j는 1에서 멈추기에
pivot이 바뀌어버린다. 그러면 [1, 4, 3, 5, 2]가 된다. pivot이 바뀌면
마지막에 18\~19줄에서 while loop이 모두 끝나고 j랑 pivot이랑 스왑될 때 left에 다른 숫자가
들어가 있어서 오류가 난다. 18\~19를 swap(v, left, j)로 바꾸어도 마찬가지이다.
pivot을 left에서 빼내면 안되기에 left+1을 하는 방법을 생각해볼 수 있다.

### 2. left+1 <a name="4-2"></a>
```cpp
int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left+1;
    int j = right;
    swap(v, pivot_index, left);
    while (i < j) {
        while (pivot > v[i]) {
            i++;
        }
        while (pivot < v[j]) {
            j--;
        }
        if (i < j) {
            swap(v, i, j);
        }
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}
```
left+1을 하면 얼핏보면 될 것 같지만, 반례가 있다. [1, 2 | 3 | 5, 4] (|는 파티션 나뉜 곳.) 여기서 [5, 4] 파티션의 5를
pivot으로 잡고 left+1부터 i와 j를 비교하면 5와 4가 바뀌지 않고 끝나서 문제가 생긴다. 고로, 다른 해법을 찾아야한다.

### 3. while(p>=v[i])i++; <a name="4-3"></a>
```cpp
int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left+1;
    int j = right;
    swap(v, pivot_index, left);
    while (i < j) {
        while (pivot >= v[i]) {
            i++;
        }
        while (pivot < v[j]) {
            j--;
        }
        if (i < j) {
            swap(v, i, j);
        }
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}
```
i를 증가시킬 때 pivot과 같은 값들은 패스하도록 설계하면 2번의 테스트 케이스를 통과한다. 하지만, 이번에는
[5, 2 | 6, 6, 6]과 같은 상태인 케이스에서 오류가 생긴다. i가 6의 가장 왼쪽 부터 시작했을 때, 같은 수는 모두 패스하기에
배열 끝보다 더 많이 넘어가서 out of bounds가 된다. 그래서 i < j를 붙여주어야한다.

### 4. i < j 추가 <a name="4-4"></a>
```cpp
int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left+1;
    int j = right;
    swap(v, pivot_index, left);
    while (i < j) {
        while (i < j && pivot >= v[i]) {
            i++;
        }
        while (pivot < v[j]) {
            j--;
        }
        if (i < j) {
            swap(v, i, j);
        }
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}
```
이렇게 i < j를 추가해주면 이제 모든 logical error은 통과이다.
흥미롭게도, pivot을 왼쪽으로 안잡고 맨 오른쪽으로 잡게되면, 반대로 `while(i < j && pivot <= v[j]) j--;`를 해주어야한다.
즉, decrment j 부분에 condition을 넣어주어야한다.

### 5. i와 j 순서 바꿔주기 <a name="4-5"></a>
```cpp
int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left+1;
    int j = right;
    swap(v, pivot_index, left);
    while (i < j) {
        while (pivot < v[j]) {
            j--;
        }
        while (i < j && pivot >= v[i]) {
            i++;
        }
        if (i < j) {
            swap(v, i, j);
        }
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}
```
이렇게 increment i 와 decrement j의 순서를 바꾸어주면 효율이 아주 조금 더 개선 된다.
예시 케이스는 [30, 20, 50, 60, 70] 이다. i가 단 한번 덜 비교 대상이 된다.
효율 개선량은 분기를 트리로 보았을 때 분기 노드 총 개수이다. 노드 총 개수는 평균적으로 $n-1$에 가깝다.
이유를 알아보자.
Complete Binary Tree는 $2^{h-1}$의 노드 수를 가진다. 우리는 Quick Sort가 랜덤으로 pivot을 취하면
평균적으로 Binary Tree의 분기가 생성된다고 볼 수 있다. 그렇기에, 평균적으로 노드 총 개수는 $2^{\log_2(n)}-1 = n-1$이고,
이 개수만큼 비교 가 줄어들 수 있다.

### 6. swap에 있는 i < j 제거 <a name="4-6"></a>
```cpp
int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left+1;
    int j = right;
    swap(v, pivot_index, left);
    while (i < j) {
        while (pivot < v[j]) {
            j--;
        }
        while (i < j && pivot >= v[i]) {
            i++;
        }
        swap(v, i, j);
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}
```
두번째 inner while loop에 i < j가 생겼기에 swap에 i < j조건문을 빼도 된다. 
비교 연산을 많이 줄일 수 있다. 정확히는 한 번의 partition 마다 n번씩 줄일 수 있다.
partition의 개수는 평균적으로 log(n)개니까 $n\log(n)$만큼의 비교 연산을 덜할 수 있다.

## 다른 풀이 세 가지 <a name="5"></a>
### 1. pivot을 오른쪽으로 두기 <a name="5-1"></a>
pivot을 오른쪽에 두면 위에서 쓴 조건과 반대로하면 된다. 그리고 18번 줄을 쓰든 comment되어 있는 16번 17번 줄을 쓰든 상관 없다.
```cpp
int partition(vector<int> &v, int left, int right) {
    int pivot_index = left + rand()%(right-left+1);
    int pivot = v[pivot_index];
    int i = left;
    int j = right;
    swap(v, pivot_index, right);
    while (i < j) {
        while (pivot > v[i]) {
            i++;
        }
        while (i < j && pivot <= v[j]) {
            j--;
        }
        swap(v, i, j);
    }
    // v[left] = v[j];
    // v[j] = pivot;
    swap(v, right, j);
    return j;
}
```
### 2. pivot을 가운데에 냅둔 상태에서 재귀 <a name="5-2"></a>
다른 곳에서 퍼온 코드인데, 맨 왼쪽이나 맨 오른쪽이랑 스왑을 하지 않고 돌린다는 점에서
테스트 케이스를 생각할 때 훨씬 간단하고 좋은 알고리즘이라고 생각한다. 정확히 최적화 관점에서까지의
시간 복잡도 차이는 모르겠다.
```cpp
// 출처: https://dpdpwl.tistory.com/46
void quickSort(int i, int j)
{
	if(i>=j) return;
	int pivot = quick[(i+j)/2];
	int left = i;
	int right = j;
	
	while(left<=right)
	{
		while(quick[left]<pivot) left++;
		while(quick[right]>pivot) right--;
		if(left<=right)
		{
			swap(quick[left],quick[right]);
			left++; right--;
		}
	}
	quickSort(i,right);
	quickSort(left,j);
}
```
### 3. 메모리를 사용하는 퀵소트 <a name="5-3"></a>
우리가 구현한 퀵소트는 메모리를 n + log(n)만큼 사용한다. 아래 알고리즘은 2n + log(n)정도 사용하는 것으로 보여진다.
```
//psuedocode
quick_sort(int left, int right) {
	pivot = random(left, right) // random from left to right
	int[] less
	int[] same
	int[] larger
	for i in list[left, right]:
		if (list[i] > pivot):
			larger.append(i)
		else if (list[i] < pivot):
			smaller.append(i)
		else:
			same.append(i)
	temp = merge(less, same, larger)
	list.replace(temp)
	quick_sort(pivot+1, right)
	quick_sort(left, pivot-1)
}
```

