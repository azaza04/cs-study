## 종류

두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법

-   (INNER) JOIN
-   LEFT OUTER JOIN
-   RIGHT OUTER JOIN
-   FULL OUTER JOIN
-   CROSS JOIN
-   SELF JOIN

### 1\. INNER JOIN

**정의:** 두 테이블의 공통된 정보만 가져오는 방식

| ![image](https://github.com/user-attachments/assets/68fcd802-010f-4a4b-a3c4-77a09fb39cca) | ![image](https://github.com/user-attachments/assets/f5c8b021-afcc-49b5-be60-799f06bb95e0) |
| --- | --- |

**결과:**

| ![image](https://github.com/user-attachments/assets/53b6a749-811f-4f3b-8857-21ccb5644af8) | ![image](https://github.com/user-attachments/assets/ce60b101-4676-458a-8e89-64f95ad0c767) |
| --- | --- |

### 2\. LEFT OUTER JOIN

**정의:** 왼쪽 테이블의 모든 정보를 가져오고, 만약 오른쪽 테이블에 해당 정보가 없으면 빈 값(NULL)을 삽입

| ![image](https://github.com/user-attachments/assets/f4c6a642-0e87-4d71-9834-7324e8cf5e8b) | ![image](https://github.com/user-attachments/assets/e4906353-72a2-4b16-8f01-80a08a99b1cb) |
| --- | --- |

**결과:**

| ![image](https://github.com/user-attachments/assets/148ec02e-fd61-4398-8e17-76f03e4499cf) | ![image](https://github.com/user-attachments/assets/068ad029-3e77-4d3f-b5ea-7bb4c28896de) |
| --- | --- |

### 3\. RIGHT OUTER JOIN

**정의:** 오른쪽 테이블의 모든 정보를 가져오고, 만약 왼쪽 테이블에 해당 정보가 없으면 빈 값(NULL)을 삽입

| ![image](https://github.com/user-attachments/assets/0c2326b1-ff31-4dfa-a544-df1c2b2fd5ee) |    ![image](https://github.com/user-attachments/assets/aee84eac-bae1-49e7-8c99-ad15a2d9df7f) |
| --- | --- |

**결과:**

| ![image](https://github.com/user-attachments/assets/ce65d4f1-5368-4171-af8f-87af9ab79d44) | ![image](https://github.com/user-attachments/assets/fff5ff12-4e30-4e26-94bb-194cc780cd4d) |
| --- | --- |

### 4\. FULL OUTER JOIN

**정의:** 두 테이블의 모든 정보를 가져오는데, 어느 한 쪽에 정보가 없으면 빈 값으로 표시

| ![image](https://github.com/user-attachments/assets/fff5ff12-4e30-4e26-94bb-194cc780cd4d) |   ![image](https://github.com/user-attachments/assets/1c05b5c5-fa5d-4af6-ad24-83bba2801645) |
| --- | --- |

**결과:**

| ![image](https://github.com/user-attachments/assets/4bd86659-47c7-42a8-a1b7-71e69d2f7556)   | ![image](https://github.com/user-attachments/assets/565be2f0-9ae8-40f6-ad7b-6fa71fc6d427) |
| --- | --- |

### 5\. CROSS JOIN

**정의:** 두 테이블의 **모든 가능한 조합**을 반환하며, 두 테이블의 Cartesian 곱을 생성하는 조인

![image](https://github.com/user-attachments/assets/0b832e2b-d697-4159-b668-82e008a44a71)

**결과:**

|    ![image](https://github.com/user-attachments/assets/50679a49-af19-4837-91a8-722860e0cc11) | ![image](https://github.com/user-attachments/assets/15852c69-423e-425f-8ff0-e879239ba7db) |
| --- | --- |

### 6\. SELF JOIN

**정의:** 같은 테이블을 자기 자신과 조인하는 방식

- 주로 계층 구조를 표현할 때 사용

![image](https://github.com/user-attachments/assets/880dd5c7-1fee-4663-9206-d7ebb9b176f8)

**결과:**

|    ![image](https://github.com/user-attachments/assets/5a9c5ef0-b390-4500-a260-42249cf8d597) | ![image](https://github.com/user-attachments/assets/b46d474f-dc82-4b06-8b8a-ed41d4fdcd8d) |
| --- | --- |