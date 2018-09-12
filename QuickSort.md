# Quick Sort
퀵 정렬은 **분할 정복(divide and conquer)** 방법으로 리스트를 정렬한다. </br>
다른 원소와의 비교만으로 정렬을 수행하는 **비교 정렬** 이며, **불안정 정렬** 에 속한다.  </br>

## Algorithm
퀵 정렬은 임의의 pivot 값을 기준으로 pivot 의 좌측에는 pivot 보다 작은 값을 두고 우측에는 pivot 보다 큰 값을 두는 것을 목적으로 하는데, 
이 행동을 pivot 을 기준으로 좌우로 이분화된 리스트들에 대해 재귀적으로 반복하며 정렬을 수행한다. </br>

1. 리스트 가운데서 하나의 원소를 고른다. -> **pivot**
2. (**divide**) pivot 앞에는 pivot 보다 작은 원소들이 오고, pivot 뒤에는 pivot 보다 값이 큰 모든 원소들이 오도록 pivot 을 기준으로 리스트를 나눈다.
3. 분할된 두 개의 작은 리스트들에 대해 재귀적으로 1-2 과정을 반복한다. 재귀는 리스트의 크기가 0 이나 1 이 될때까지 반복한다. 

> 재귀 호출이 한 번 진행될 때마다 하나의 원소는 최종적으로 위치가 정해지므로, 알고리즘이 반드시 끝난다는 것을 보장할 수 있다. 

### Time Complexity 
퀵 정렬은 *O(n log n)* 의 시간복잡도를 가진다. 
> 퀵정렬의 내부 루프는 지역성의 원리에 따라 CPU 캐시 히트율이 높아지도록 설계 되어있기 때문에 일반적으로 다른 *O(n log n)*  알고리즘에 비해 훨씬 빠르게 동작한다.

## Code

### Java
````java
public void partiion(int[] arr, int l, int r) {
  // pivot 을 결정하는 기준은 달라질 수 있음
  int pivot = arr[(l + r) / 2];
  
  while (left < right) {
    while ((arr[left] < pivot) && (left < right))
      left++;
    while ((arr[right] > pivot) && (left < right))
      right--;
  
    if (left < right) {
      int temp = arr[left];
      arr[left] = arr[right];
      arr[right] = temp;
    }
  }
  
  return left;
}

public void quickSort(int[] arr, int l, int r) {
  if (l < r) {
    int idx = partition(arr, left, right);
    quickSort(arr, left, idx - 1);
    quickSort(arr, idx + 1, right);
  }
}
````
> java 에서는 `java.util.Arrays` 로 primitive 타입을 정렬하고자 할 때, quick sort 를 사용한다. 
