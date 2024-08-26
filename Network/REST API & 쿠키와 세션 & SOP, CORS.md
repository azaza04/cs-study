# [Network] REST API & 쿠키와 세션 & SOP, CORS

# REST API

### REST(REpresentational State Transfer)

클라이언트-서버 간의 상호작용을 자원(Resource) 기반으로 구조화한 아키텍처 스타일

자원은 URI로 표현되며, 모든 상태는 HTTP 메서드를 통해 관리됨

- 자원(Resource) - URI
- 행위(Verb) - HTTP METHOD

<br/>

### REST의 특징

- **캐시 가능 (Cacheable)**
    - HTTP라는 기존 웹표준을 그대로 사용하므로, 웹에서 사용하는 기존 인프라를 그대로 활용할 수 있고, 따라서 HTTP가 가진 캐싱 기능을 그대로 사용할 수 있음
    - HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하여 구현
- **상태 관리와 무상태성 (Statelessness)**
    - 서버는 클라이언트의 요청을 독립적으로 처리하며, 이전 요청의 상태를 저장하지 않습니다. 이를 통해 확장성과 성능이 개선됩니다. 상태는 클라이언트 측에서 관리되며, 필요한 정보는 매 요청마다 포함되어야 합니다.
- **자체 표현 구조 (Self-descriptiveness)**
    - REST API 메시지만 보고도 어떤 기능을 해야 하는지 쉽게 이해 할 수 있는 구조
- **계층형 구조**
    - 다중 계층으로 구성될 수 있고, 보안 / 로드 밸런싱 / 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고, PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있음

<br/>

### RESTful API 설계 원칙

<img width="760" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6 04 18" src="https://github.com/user-attachments/assets/6330cd77-9d9c-4eae-8ba2-da0235860eac">

> 1. URI는 정보의 자원을 표현해야 한다.
> 
> 
> 2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.
> 
- 슬래시 구분자(/)는 계층 관계를 나타냄
- URI 마지막 문자로 슬래시(/)를 포함하지 않는다
    - ref) 장고 ↔ 스프링
- 불가피하게 긴 URI 경로를 사용하게 된다면 하이픈(-)을 사용
- 언더바(_)는 사용하지 않는다
- 대문자보다는 소문자를 사용
- 파일 확장자는 URI에 포함시키지 않음

```bash
// 수정 전
GET /members/delete/1

// 수정 후
DELETE /members/1
```

<br/>

### GET과 POST의 차이

<img width="764" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6 21 30" src="https://github.com/user-attachments/assets/efed4c96-b93e-4ecb-8789-c6f6e207e126">

- 멱등성(Idempotency) : **동일한 연산을 여러 번 수행해도 결과가 달라지지 않는 성질**

<br/>

### PATCH과 PUT의 차이

- [**PATCH**](https://tools.ietf.org/html/rfc5789), which is used to apply **partial modifications** to a resource
- [**PUT**](https://tools.ietf.org/html/rfc7231#section-4.3.4) method requests that the state of the target resource be created or replaced with the state defined by the representation enclosed in the request message payload

PUT 요청 시 요청을 일부분만 보낸 경우 나머지는 default 값으로 수정되는 게 원칙이므로 바뀌지 않는 속성도 모두 보내야 함

자원의 일부를 수정할 때는 PATCH를, 전체적인 수정이 필요할 때는 PUT을 이용하는 것이 적절함

<br/>

---

<br/>

# 쿠키와 세션

## 쿠키

![image](https://github.com/user-attachments/assets/17703402-5d64-4a31-8ceb-f5d0a96bf6a2)

- **클라이언트(브라우저)에 저장**되는 키-값 형태의 작은 데이터 파일
- 각 쿠키는 이름, 값, 만료 날짜, 도메인 등의 속성을 가짐
    - 이름 : 각각의 쿠키를 구별하는 데 사용되는 이름
    - 값 : 쿠키의 이름과 관련된 값
    - 유효시간 : 쿠키의 유지시간
    - 도메인 : 쿠키를 전송할 도메인
    - 경로 : 쿠키를 전송할 요청 경로
- 쿠키는 특정 도메인과 경로에 대해서만 유효
    - [https://www.naver.com](https://www.naver.com) 의 쿠키는 [https://google.com](https://google.com) 에서 유효하지 않음
- Secure 속성은 HTTPS를 통해서만 전송되도록 하며, HttpOnly 속성은 자바스크립트에서 접근하지 못하도록 설정해 보안을 강화
- 사용자가 따로 설정하지 않아도 브라우저가 Request Header에 쿠키를 자동으로 담아서 요청함
- **쿠키의 사용 예시:** 로그인 상태 유지, 장바구니 상태 저장, 사용자 추적(Analytics) 등의 용도로 사용됨

<br/>

## 세션

![image 1](https://github.com/user-attachments/assets/0d990b2a-a015-4dd4-996a-31ef495846c4)

- 서버 측에 저장된 사용자 데이터
- 클라이언트가 서버와 지속적으로 상호작용할 수 있도록 식별자(Session ID)를 사용해 관리
- 클라이언트는 세션 ID를 쿠키로 서버에 전달하여 자신의 상태를 유지

<br/>

**⇒ 세션과 쿠키의 차이점**

- 쿠키는 클라이언트 측에 데이터를 저장하고, 세션은 서버 측에 저장
- 쿠키는 주로 간단한 정보 저장에 사용되며, 세션은 더 많은 정보를 안전하게 관리하는 데 사용됨

<br/>

### **쿠키와 세션의 보안 문제**

- **세션 하이재킹 (Session Hijacking)과 방어 기법:** 공격자가 세션 ID를 탈취해 사용자의 권한으로 서버에 접근할 수 있다. 이를 방지하기 위해 SSL/TLS를 사용하고, 세션 ID를 정기적으로 재생성하며, 세션 만료 시간을 짧게 설정한다.
- **쿠키와 세션의 취약점 및 대응 방법:** XSS(Cross-Site Scripting)를 통해 쿠키가 탈취될 수 있으므로 HttpOnly 쿠키를 사용하고, CSRF(Cross-Site Request Forgery) 공격을 방지하기 위해 CSRF 토큰을 사용한다.

<br/>

## Session 기반 인증과 JWT 비교

### Session 기반 인증 방식

<img width="704" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6 27 46" src="https://github.com/user-attachments/assets/76af2478-5e4b-4e84-9134-12b0f10b3f36">


- 장점
    - 서버 측에서 유저의 인증 정보를 관리하기 때문에, 보안성이 높음
    - 유저의 인증 정보를 서버 측에서 유지하기 때문에, 클라이언트 측에서 인증 정보를 저장하고 관리하는 토큰 방식과 달리 쉽게 탈취될 가능성이 적음
    - 클라이언트 측에서 인증 정보를 저장하고 관리하는 토큰 방식과 달리, 새로고침이나 브라우저 종료 등으로 인한 세션 만료 기간 설정이 가능
- 단점:
    - 클라이언트가 요청할 때마다 서버에서 세션 정보를 확인해야 하기 때문에, 서버 부하가 큼
    - 서버의 메모리나 데이터베이스에 세션 정보를 저장하기 때문에 확장성이 떨어질 수 있음

<br/>

### JWT 방식

<img width="693" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6 27 59" src="https://github.com/user-attachments/assets/1465c458-5681-4483-9ab9-43376a26cfb4">

장점:

- 클라이언트 측에서 인증 정보를 저장하고 관리하기 때문에, 서버 측에서 유저의 인증 정보를 저장하고 관리하는 세션 방식보다 서버 부하가 적음
- 클라이언트 측에서 인증 정보를 저장하고 관리하기 때문에, 서버와의 통신이 감소
- JSON Web Token(JWT)등을 사용하면, 토큰 내에 필요한 정보를 담을 수 있음

단점:

- 토큰을 탈취당하면, 탈취한 사람이 해당 유저처럼 행동할 수 있기 때문에 토큰 탈취 대응 로직이 필요함

<br/>

# 토큰을 안전하게 저장하는 방법

- HttpOnly 쿠키 사용 : JavaScript로 접근할 수 없도록 설정되어 있어, XSS(Cross-Site Scripting) 공격 등으로부터 보호됨
- HTTPS 사용 : HTTPS 프로토콜을 사용하여 클라이언트와 서버 간의 통신을 암호화하면, 중간자 공격 등으로부터 보호됩니다.
- 토큰 암호화 : 토큰을 암호화하여 저장하면, 토큰을 탈취해도 해당 토큰을 해독할 수 없으므로 보안성이 높아집니다.
- 브라우저 저장소 사용: 토큰을 브라우저 저장소에 저장하면, 서버와 통신할 때마다 해당 토큰을 전송해야 하므로 보안성이 떨어지지만, 사용자 편의성이 좋아서 많이 사용된다.

<br/>

---

<br/>

# SOP, CORS

## 동일 출처 정책 (SOP, Same-Origin Policy)

- 웹 페이지가 다른 출처의 리소스에 접근하는 것을 제한하는 보안 정책
- 악의적인 스크립트가 민감한 데이터에 접근하지 못하도록 보호합니다.
- Origin = Protocol, Host, Port
    - SOP는 프로토콜, 호스트, 포트가 동일한 경우에만 리소스 접근을 허용

![image 2](https://github.com/user-attachments/assets/e90494c6-306b-4aca-96d2-5bf085ae9c1b)

![2f611126-6c66-4957-a7fb-00c404f84628](https://github.com/user-attachments/assets/afc2d0e6-2af0-425d-8bdd-0cbdb5d78695)


<br/>

## CORS (Cross-Origin Resource Sharing)

CORS(Cross-Origin Resource Sharing)는 웹 애플리케이션이 서로 다른 도메인에서 자원을 요청할 수 있도록 허용하는 웹 보안 표준

<br/>

### **CORS의 동작 원리**

- **Preflight 요청**: 서버가 특정 헤더나 메소드를 사용한 요청을 허용하는지 확인하기 위해 브라우저는 본 요청을 보내기 전에 OPTIONS 메소드로 사전 요청을 함
    - Preflight 요청은 다음과 같은 정보를 포함
        - HTTP 메소드 (예: GET, POST, PUT, DELETE)
        - 요청 헤더 (예: Content-Type, Authorization)
- **응답 헤더**: 서버는 CORS 정책을 응답 헤더에 명시하여 브라우저가 요청을 허용할지 결정
    - `Access-Control-Allow-Origin`: 허용된 출처를 지정 (예: ‘*’, 특정 도메인)
    - `Access-Control-Allow-Methods`: 허용된 HTTP 메소드를 지정 (예: `GET, POST`)
    - `Access-Control-Allow-Headers`: 허용된 요청 헤더를 지정 (예: `Content-Type`)
    - `Access-Control-Allow-Credentials`: 자격 증명(쿠키, 인증 헤더) 포함 여부를 지정
- **Simple Requests**: 단순 요청은 Preflight 없이 바로 서버에 전달됨 GET, POST, HEAD 메소드와 특정 헤더만 허용
- **Non-Simple Requests**: Preflight 요청이 필요한 복잡한 요청으로, DELETE, PUT 메소드나 특정 헤더를 사용할 때 발생

<br/>

### **CORS 설정 방법**

- **서버 측 설정**: 서버에서 CORS를 허용하는 응답 헤더를 추가
    - 예: `Access-Control-Allow-Origin: https://example.com`
- **프레임워크별 설정**:
    - **Express.js**: `cors` 미들웨어를 사용하여 CORS를 간편하게 설정
    - **Django**: `django-cors-headers` 패키지 사용하여 CORS 설정
    - **Spring Boot**:
        - `@CrossOrigin` 어노테이션을 사용하여 특정 컨트롤러나 메소드에서 CORS를 허용
        
        ```java
        @CrossOrigin(origins = "http://example.com")
        @GetMapping("/api/resource")
        public ResponseEntity<String> getResource() {
            return ResponseEntity.ok("Hello!");
        }
        ```
        
        - `WebMvcConfigurer` 인터페이스를 구현한 Config 빈을 등록하여 Spring 서버 전역적으로 설정
        
        ```java
        // Spring 서버 전역적으로 CORS 설정
        @Configuration
        public class WebConfig implements WebMvcConfigurer {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("*") // 허용할 출처 : 특정 도메인만 받을 수 있음
                        .allowedMethods("GET", "POST") // 허용할 HTTP method
                        .allowCredentials(true); // 쿠키 인증 요청 허용
            }
        }
        ```
        

<br/>

# 질문

<br/>

# 출처

- [https://inpa.tistory.com/entry/WEB-🌐-REST-API-정리](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-REST-API-%EC%A0%95%EB%A6%AC)
- [https://blog.naver.com/shino1025/221568544633](https://blog.naver.com/shino1025/221568544633)
- [https://cordcat.tistory.com/108](https://cordcat.tistory.com/108)
