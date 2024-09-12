## DB index

![image](https://github.com/user-attachments/assets/45e40a22-e62f-425d-8d4b-bb96f2a51750)

-   **정의**: 데이터를 빠르게 검색할 수 있도록 도와주는 데이터 구조
-   **Why?**: 데이터를 빠르게 검색하고 조회 성능을 개선하기 위해 사용
    -   책의 목차처럼 특정 데이터의 위치를 미리 알고 빠르게 찾을 수 있게 한다.

## 언제 사용?

#### **사용해야 할 때:**

-   자주 검색하거나 정렬해야 하는 컬럼이 있을 때
-   WHERE, JOIN, ORDER BY, GROUP BY 절에 자주 사용되는 컬럼
-   데이터 양이 많아질수록 검색 성능이 중요한 경우

#### **사용하지 말아야 할 때:**

-   자주 업데이트, 삽입, 삭제가 발생하는 컬럼 (인덱스 유지 비용이 높음)
-   작은 테이블 (인덱스 효과가 크지 않음)

## 인덱스 종류

#### dense vs sparse

-   dense : 데이터 파일의 모든 값에 대한 인덱스 항목이 존재하는 것을 의미
-   sparse : 데이터 파일의 일부 값에 대해서만 인덱스 항목이 존재 -> 검색시 추가적인 탐색이 필요

### 유형

#### Primary index

-   key 속성을 인덱스로 사용하는 경우(정렬o)(중복x)
-   해당 값이 저장된 블록의 위치를 인덱스 파일에 저장 (sparse)

![image](https://github.com/user-attachments/assets/aeb547aa-734c-4dcb-9b3d-dc383dafce67)

#### Clustering index

-   정렬된 일반 속성을 인덱스로 사용하는 경우(정렬o)(중복o)
-   첫번째 값이 저장되어 있는 블록의 위치를 인덱스 파일에 저장 (sparse)

![image](https://github.com/user-attachments/assets/b528d153-972b-4b61-9014-25deafcb82ca)


#### Secondary index

-   정렬되지않은 속성을 인덱스로 사용하는 경우, 해당 값이 저장된 레코드의 위치를 인덱스 파일에 저장 (dense)
-   key값과 nonkey값의 경우가 다름
    -   key값 : 모든 레코드의 위치를 단순히 인덱스 파일에 저장하면 됨
    -   nonkey값 : 얘는 심지어 중복된 값도 존재
        -    < A인덱스, 10>  /  <A인덱스, 15> .....
        -    <A인덱스, 10, 15, 20... >
        -   중간에 레코드 포인터 블록을 삽입하는 방법 ( 아래 사진 )

|    ![image](https://github.com/user-attachments/assets/bcefd098-dc14-4c5e-ab1b-804db655a62e)      Key값 |    ![image](https://github.com/user-attachments/assets/ec018e28-f9be-44a9-bf45-851d73ffe7d0)   nonkey값 |
| --- | --- |

## 인덱스 구현 방법

#### **B-Tree 인덱스**

![image](https://github.com/user-attachments/assets/a2316d0a-a193-4a1d-9c42-464f1d943371)

-   **정의**: 가장 일반적인 인덱스 유형으로, 인덱스를 트리 구조로 저장
-   **시간복잡도**: O(logN)
-   **장점**:
    -   범위 검색과 정렬된 데이터 접근에 적합
    -   균형을 유지하므로, 성능이 안정적
-   **단점**: 
    -   삽입과 삭제 시 리밸런싱이 필요하여 오버헤드 발생

#### **Hash 인덱스**

![image](https://github.com/user-attachments/assets/fc8dafd0-0cdd-4aa5-80d8-74bad70c2f83)

-   **정의**: 해시 테이블을 이용해 특정 키의 위치를 빠르게 찾을 수 있는 인덱스
-   **시간복잡도**: O(1)
-   **장점**:
    -   정확한 키 일치 검색에 매우 빠름
    -   구현이 간단하고 성능이 우수
-   **단점**:
    -   범위 검색이나 정렬에는 부적합
    -   데이터가 많아지면 해시 충돌 처리도 추가적으로 진행해야함

#### **Bitmap 인덱스**

![image](https://github.com/user-attachments/assets/0b5abc2f-0d8d-45e0-97a6-02a479ce5d01)

-   **정의**: 비트맵으로 데이터를 표현
-   **시간복잡도**: O(n) but. 비트 연산이라 매우 빠름
-   **장점**:
    -   중복 값이 많은 경우 빠른 성능 발휘
    -   저장 공간 효율적
-   **단점**:
    -   데이터가 자주 변경되면 인덱스 유지 비용이 큼
    -   고유한 값이 많을 경우 비트맵 크기가 커짐