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

## 거품 정렬(Bubble Sort)

![image](https://blog.kakaocdn.net/dn/DrEny/btsJPLekcyg/F6KCTcfWkZAl16eoOPgrF0/img.gif)

-   **정의**: 서로 인접한 두 원소의 대소를 비교하고, 조건에 맞지 않다면 자리를 교환하며 정렬하는 알고리즘
-   **특징**:
    -   **제자리 정렬(in-place sorting)** - 기존 배열 내에서 인접한 원소들을 교환하여 정렬
    -   **안정 정렬(Stable Sort)** - 인접한 두 원소가 같을 경우 교환 발생x

#### 자바 코드

```Java
void bubbleSort(int[] arr) {
    int temp = 0;
    for(int i = 0; i < arr.length; i++) {       // 1.
        for(int j= 1 ; j < arr.length-i; j++) { // 2.
            if(arr[j-1] > arr[j]) {             // 3.
                // swap(arr[j-1], arr[j])
                temp = arr[j-1];
                arr[j-1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    System.out.println(Arrays.toString(arr));
}
```

1.  제외될 원소의 갯수를 의미. 1회전이 끝난 후, 배열의 마지막 위치에는 가장 큰 원소가 위치하기 때문에 하나씩 증가
2.  원소를 비교할 index를 뽑을 반복문. j는 현재 원소를 가리키고, j-1은 이전 원소를 가리키게 되므로, j는 1부터 시작
3.  현재 가르키고 있는 두 원소의 대소를 비교. 해당 코드는 오름차순 정렬이므로 현재 원소보다 이전 원소가 더 크다면 이전 원소가 뒤로 가야하므로 서로 자리를 교환

## 선택 정렬(Selection Sort)

![image](https://blog.kakaocdn.net/dn/tISOz/btsJPRL3MfX/vp6whmGuRp3kKLoUN41Nn0/img.gif)

-   **정의**: 해당 자리를 **선택**하고, 그 자리에 오는 값(최대 or 최소)을 찾는 알고리즘
-   **특징**:
    -   **제자리 정렬(in-place sorting)** \- 기존 배열 안에서 값들의 위치만 바꾸며 정렬을 진행
    -   **불안정 정렬(Unstable Sort)** \- 위치를 교환하는 과정에서 값의 순서가 바뀔 수 있음
        -   전: \[**(3, 'A')**, (2, 'D'), **(3, 'C')**, (1, 'B')\]
        -   후: \[(1, 'B'), (2, 'D'), **(3, 'C')**,  **(3, 'A')**\]

#### 자바 코드

```Java
void selectionSort(int[] arr) {
    int indexMin, temp;
    for (int i = 0; i < arr.length-1; i++) {        // 1.
        indexMin = i;
        for (int j = i + 1; j < arr.length; j++) {  // 2.
            if (arr[j] < arr[indexMin]) {           // 3.
                indexMin = j;
            }
        }
        // 4. swap(arr[indexMin], arr[i])
        temp = arr[indexMin];
        arr[indexMin] = arr[i];
        arr[i] = temp;
  }
  System.out.println(Arrays.toString(arr));
}
```

1.  우선, 위치(index)를 **선택**
2.  i+1번째 원소부터 선택한 위치(index)의 값과 비교를 시작
3.  오름차순이므로 현재 선택한 자리에 있는 값보다 순회하고 있는 값이 작다면, 위치(index)를 갱신
4.  '2'번 반복문이 끝난 뒤에는 indexMin에 '1'번에서 선택한 위치(index)에 들어가야하는 값의 위치(index)를 갖고 있으므로 서로 교환(swap)

## 삽입 정렬(Insertion Sort)

![image](https://blog.kakaocdn.net/dn/bDS7Ty/btsJPQsQrUi/LzWa941lSpE9k4Cke7zmu0/img.gif)

-   **정의**: 2번째 원소부터 시작하여 그 앞(왼쪽)의 원소들과 비교하여 삽입할 위치를 지정한 후, 원소를 뒤로 옮기고 지정된 자리에 자료를 삽입 하여 정렬하는 알고리즘
-   **특징**:
    -   **제자리 정렬(in-place sorting)** \- 배열 안에서 직접 요소들을 이동시키며 정렬
    -   **안정 정렬(Stable Sort)** \- 위치를 전부 이동시키면서 삽입하기에 순서 유지

#### 자바 코드

```Java
void bubbleSort(int[] arr) {
    int temp = 0;
    for(int i = 0; i < arr.length; i++) {       // 1.
        for(int j= 1 ; j < arr.length-i; j++) { // 2.
            if(arr[j-1] > arr[j]) {             // 3.
                // swap(arr[j-1], arr[j])
                temp = arr[j-1];
                arr[j-1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    System.out.println(Arrays.toString(arr));
}
```

1.  제외될 원소의 갯수를 의미. 1회전이 끝난 후, 배열의 마지막 위치에는 가장 큰 원소가 위치하기 때문에 하나씩 증가
2.  원소를 비교할 index를 뽑을 반복문. j는 현재 원소를 가리키고, j-1은 이전 원소를 가리키게 되므로, j는 1부터 시작
3.  현재 가르키고 있는 두 원소의 대소를 비교. 해당 코드는 오름차순 정렬이므로 현재 원소보다 이전 원소가 더 크다면 이전 원소가 뒤로 가야하므로 서로 자리를 교환