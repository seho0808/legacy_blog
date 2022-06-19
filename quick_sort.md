---
title: Quick Sort
parent: Algorithms
has_children: false
---
## Quick Sort (퀵소트 / 퀵정렬)

이 글은 퀵정렬의 원리에 대해 설명하지 않는다. 최적화와 구현 방식들에 대해서만 이야기한다.

## 최적화 완료된 알고리즘
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

## 최적화 순서
### 1. 첫 어프로치
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

### 2. left+1
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

### 3. while(p>=v[i])i++;
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

### 4. i < j 추가
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

### 5. i와 j 순서 바꿔주기
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





