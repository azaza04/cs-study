| **알고리즘** | **최적 시간** | **평균 시간** | **최악 시간** | **제자리 정렬** | **안정 정렬** |
| --- | --- | --- | --- | --- | --- |
| 거품 정렬 | O(n) | O(n²) | O(n²) | O | O |
| 선택 정렬 | O(n²) | O(n²) | O(n²) | O | X |
| 삽입 정렬 | O(n) | O(n²) | O(n²) | O | O |
| 퀵 정렬 | O(n log n) | O(n log n) | O(n²) | O | X |
| 병합 정렬 | O(n log n) | O(n log n) | O(n log n) | X | O |
| 힙 정렬 | O(n log n) | O(n log n) | O(n log n) | O | X |
| 기수 정렬 | O(nk) | O(nk) | O(nk) | X | O |
| 계수 정렬 | O(n + k) | O(n + k) | O(n + k) | X | O |

## In-place & Stable Sort

-   **제자리 정렬(in-place sorting)**: 추가적인 메모리 공간을 거의 사용하지 않고, 입력 배열 자체에서 정렬을 수행하는 알고리즘을 의미
    -   O(1) 또는 O(log n) 정도의 보조 메모리만 사용하며, 기존 데이터를 재배치하는 방식으로 작동
-   **안정 정렬(Stable Sort)**: 동일한 값의 요소들이 정렬 전후의 순서가 바뀌지 않는 정렬 알고리즘을 의미
    -   즉, 값이 같은 데이터가 있을 때, 원래의 입력 순서가 보존

## 퀵 정렬(Quick Sort)

![image](https://blog.kakaocdn.net/dn/eHbP1a/btsJQJl5VaT/4fdKPC5AsrUjpLX0kohx5K/img.gif)

-   **정의**: 기준값(피벗)을 설정하고, 이를 기준으로 왼쪽은 작은 값, 오른쪽은 큰 값으로 분할하여 정렬하는 알고리즘
-   **특징**:
    -   **분할 정복 알고리즘(Divide and Conquer)** - 피벗을 중심으로 배열을 나누어 정렬
    -   **제자리 정렬(in-place sorting)** - 피벗을 기준으로 값을 교환하는 방식으로 정렬하므로, 추가적인 메모리 사용X
    -   **불안정 정렬(Unstable Sort)** - 동일한 값의 순서가 보장되지 않음

#### 최악의 경우와 개선 방법

-   **when?**: 피벗 값이 부분 배열에서 최소나 최대값으로 지정되어 파티션이 나누어지지 않았을 때 ( 예: 이미 정렬이 되어 있는 경우 )
-   **How?**:  배열에서 가장 앞에 있는 값과 중간값을 교환해준다면 확률적으로나마 시간복잡도 O(nlog₂n)으로 개선 가능

#### 자바 코드

```Java
public void quickSort(int[] array, int left, int right) {
    if(left >= right) return;
    
    // 분할 
    int pivot = partition(); 
    
    // 피벗은 제외한 2개의 부분 배열을 대상으로 순환 호출
    quickSort(array, left, pivot-1);  // 정복(Conquer)
    quickSort(array, pivot+1, right); // 정복(Conquer)
}

public int partition(int[] array, int left, int right) {
    /**
    // 최악의 경우, 개선 방법
    int mid = (left + right) / 2;
    swap(array, left, mid);
    */
    
    int pivot = array[left]; // 가장 왼쪽값을 피벗으로 설정
    int i = left, j = right;
    
    while(i < j) {
        while(pivot < array[j]) {
            j--;
        }
        while(i < j && pivot >= array[i]){
            i++;
        }
        swap(array, i, j);
    }
    array[left] = array[i];
    array[i] = pivot;
    
    return i;
}
```

## 병합 정렬(Merge Sort)

![image](https://blog.kakaocdn.net/dn/dQSKlQ/btsJQ3YUUce/ImOQY5cF2uALkCkDMA7VgK/img.gif)

-   **정의**: 배열을 절반으로 분할한 후, 각각을 재귀적으로 정렬하고 합치는 방식의 알고리즘
-   **특징**:
    -   **분할 정복 알고리즘(Divide and Conquer)** - 배열을 분할하고 병합하는 과정을 반복
    -   **안정 정렬(Stable Sort)** - 같은 값의 순서가 보장됨
    -   **추가 메모리 사용** - 배열을 병합하기 위한 추가 공간 필요

#### 자바 코드

```Java
public void mergeSort(int[] array, int left, int right) {
    
    if(left < right) {
        int mid = (left + right) / 2;
        
        mergeSort(array, left, mid);
        mergeSort(array, mid+1, right);
        merge(array, left, mid, right);
    }
    
}


public static void merge(int[] array, int left, int mid, int right) {
    int[] L = Arrays.copyOfRange(array, left, mid + 1);
    int[] R = Arrays.copyOfRange(array, mid + 1, right + 1);
    
    int i = 0, j = 0, k = left;
    int ll = L.length, rl = R.length;
    
    while(i < ll && j < rl) {
        if(L[i] <= R[j]) {
            array[k] = L[i++];
        }
        else {
            array[k] = R[j++];
        }
        k++;
    }
    
    // remain
    while(i < ll) {
        array[k++] = L[i++];
    }
    while(j < rl) {
        array[k++] = R[j++];
    }
}
```

## 힙 정렬(Heap Sort)

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeEI4U9%2FbtsJQcbs7d5%2FakBgi5ITx5IwPjoKCdyqXk%2Fimg.png)
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGEJ3o%2FbtsJQEkVlCE%2FEKaahwdl5krFkVKxpIkc4k%2Fimg.png)

-   **정의**: 힙 자료구조(최대 힙 또는 최소 힙)를 이용해 정렬하는 알고리즘
-   **특징**:
    -   **완전 이진 트리 기반** - 최대 힙이나 최소 힙을 구성하여 정렬
    -   **제자리 정렬(in-place sorting)** - 추가적인 메모리 없이 정렬 가능
    -   **불안정 정렬(Unstable Sort)** - 동일한 값의 순서가 유지되지 않음
-   **과정**:
    1.  최대 힙을 구성
    2.  현재 힙 루트는 가장 큰 값이 존재함. 루트의 값을 마지막 요소와 바꾼 후, 힙의 사이즈 하나 줄임
    3.  힙의 사이즈가 1보다 크면 위 과정 반복

#### 자바 코드

```Java
public void heapSort(int[] array) {
    int n = array.length;
    
    // max heap 초기화
    for (int i = n/2-1; i>=0; i--){
        heapify(array, n, i); // 1
    }
    
    // extract 연산
    for (int i = n-1; i>0; i--) {
        swap(array, 0, i); 
        heapify(array, i, 0); // 2
    }
}
```

1.  일반 배열을 힙으로 구성하는 역할
2.  요소가 하나 제거된 이후에 다시 최대 힙을 구성하는 연산

```
public void heapify(int array[], int n, int i) {
    int p = i;
    int l = i*2 + 1;
    int r = i*2 + 2;
    
    //왼쪽 자식노드
    if (l < n && array[p] < array[l]) {
        p = l;
    }
    //오른쪽 자식노드
    if (r < n && array[p] < array[r]) {
        p = r;
    }
    
    //부모노드 < 자식노드
    if(i != p) {
        swap(array, p, i);
        heapify(array, n, p);
    }
}
```

-   다시 최대 힙을 구성할 때까지 부모 노드와 자식 노드를 swap하며 재귀 진행