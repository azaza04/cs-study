정의: 해커에 의해 조작된 SQL 쿼리문이 데이터베이스에 그대로 전달되어 비정상적 명령을 실행시키는 공격 기법

## 공격 방법

### Basic SQL Injection

-   **정의**: 쿼리 문자열에 악성 SQL 코드 삽입
-   **예시**: 항상 참이 되는 조건을 삽입해 인증을 우회하는 경우 (' OR '1'='1 )

```
SELECT * FROM users WHERE username = 'input' AND password = 'input';
```

```
SELECT * FROM users WHERE username = 'jang' OR '1'='1'; -- AND password = 'input';
```

### Union-Based SQL Injection

-   **정의**: UNION SQL 연산자를 사용하여 추가 데이터를 반환받음
    -   단, 자료형이 동일해야함

-   **예시**: 상품 정보를 가져오는 쿼리를 통해, 유저 정보를 추출하는 경우

```
SELECT name, price FROM products WHERE id = 'input';
```

```
SELECT name, price FROM products WHERE id = '1 UNION SELECT username, password FROM users --';
```

### Time-Based SQL Injection

-   **정의**: 쿼리 응답 시간을 지연시켜 정보 추출
-   **예시**: 조건문을 통해 지연을 발생시켜 조건이 참인지 거짓인지 정보를 추출 
    -   비밀번호 길이가 5자보다 길면, sleep함수가 발동하여 응답이 지연
-   \= 응답이 지연되면, 비밀번호 길이가 5자보다 길다는 의미

```
SELECT * FROM users WHERE user_id = 'input';
```

```
SELECT * FROM users WHERE user_id = '1' AND IF((SELECT LENGTH(password) FROM users WHERE user_id = 'admin') > 5, SLEEP(5), 0) --';
```

### Error-Based SQL Injection

-   **정의**: 오류 메시지를 통해 DB정보를 추출하는 경우

## 방어 방법

### 입력 값에 대한 검증

유저에게 받은 값을 직접 SQL로 넘기면 안된다

-   프론트단에서 JS로 폼 입력값을 한 번 검증
-   서버측에서 또 다시 입력값을 검증

### Prepared statement

-   사용자가 입력한 값을 SQL 쿼리에 직접 넣지 않고, 미리 준비된 쿼리에 안전하게 값을 채워 넣는 방식
    -   이렇게 하면, 악성 코드를 입력해도 코드로 인식하지 않음
-   p.s) 스프링 JPQL도 기본적으로 prepared statement기반

### Error Message 노출 금지

-   데이터베이스와 연결할 때 문제가 생기면 에러 메시지를 보여줄 수 있는데, 이때 해커에게 중요한 정보가 노출될 수 있다.
-   따라서 에러가 발생해도 구체적인 내용은 숨긴다.