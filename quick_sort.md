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
        swap(v, i, j);
    }
    v[left] = v[j];
    v[j] = pivot;
    return j;
}
```
이 코드의 반례는 [5, 4, 3, 2, 1]이다. 첫 루프에서 2를 pivot으로 잡으면
[2, 4, 3, 5, 1]이 된다. 여기서 i가 2에서 멈추고 j는 1에서 멈추기에
pivot이 바뀌어버린다. 그러면 [1, 4, 3, 5, 2]가 된다. pivot이 바뀌면
마지막에 16~17줄에서 while loop이 모두 끝나고 j랑 pivot이랑 스왑될 때 left에 다른 숫자가
들어가 있어서 오류가 난다. 16~17을 swap(v, left, j)로 바꾸어도 마찬가지이다.







