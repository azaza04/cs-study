## 기초

주소창에 url을 입력했을때 통신의 흐름에 대해 알아보는 것

### IP 주소 vs Domain Name

-   IP주소: 네트워크에서 컴퓨터 장치를 식별하기 위해 사용하는 번호 ( 128.0.0.1 )
-   Domain Name: IP 주소를 문자로 표현한 주소 ( naver.com )

## 웹 통신 흐름

![image](https://github.com/user-attachments/assets/de72ac23-651c-428e-8e88-f8d1825080eb)

1\. 사용자가 웹 브라우저를 통해 **URL**을 입력한다.

2\. 입력된 URL 중 도메인 네임을 **DNS** 서버에서 검색한다.

3\. DNS 서버에서 해당 도메인 네임에 해당하는 **IP 주소**를 찾아 사용자가 입력한 URL 정보와 함께 전달한다.

4\. 웹 페이지 URL 정보와 전달받은 IP 주소를 이용해 **HTTP 요청(= HTTP Request)** 메시지를 생성한다.

5\. 요청은 **TCP**를 통해 **서버**로 전송된다.

6\. 서버는 클라이언트의 요청을 받고 **응답(= HTTP Response)**을 전송한다.

## DNS(Domain Name System)

정의: 문자열 인터넷 주소(URL)을 IP 주소로 변환해주는 곳

### 도메인 구조

![image](https://github.com/user-attachments/assets/4b9556f5-b434-4a6e-b524-782d97e176f7)

### DNS 구성 요소

**1\. 도메인 네임 스페이스 (Domain Name Space):** 네임 스페이스는 DNS가 관리하는 계층적 구조를 의미

-   (Root, TLD, Sub Domain 등)



**2\. 네임서버 (Name Server):** 네임 스페이스의 트리 구조에 대한 정보를 가지고 있는 서버

-   도메인 이름을 IP주소로 변환하는 것을 네임 서비스라고 하며, 아래에서 설명할 리졸버로부터 요청받은 도메인 이름에 대한 IP 정보를 다시 리졸버로 전달해주는 역할을 수행



**3\. 리졸버 (Resolver):** 웹 브라우저와 같이 DNS 클라이언트의 요청을 네임 서버로 전달하고 네임서버로부터 정보를 받아 클라이언트에게 제공하는 역할을 수행  
\= Public DNS Server / = Recursive(재귀적) DNS Server


**4\. Authoritative(권한있는) Name Server:** 실제 개인 도메인과 IP 주소의 상관관계가 기록/저장/변경되는 서버로, 일반적으로 호스팅/도메인 업체의 네임서버가 해당. 최종적으로 정보를 얻게 되는 네임 서버

### DNS 동작 원리

![image](https://github.com/user-attachments/assets/e517bab5-69b8-4d19-9fdf-78966feb1e55)

**1\. 도메인 이름 입력**: 사용자가 웹 브라우저에 www.example.com과 같은 도메인 이름을 입력

**2\. DNS 캐시 확인**: 브라우저는 먼저 로컬 캐시에 이 도메인에 대한 IP 주소가 저장되어 있는지 확인 

-   브라우저 자체 캐시, 운영 체제의 DNS 캐시, 로컬 네트워크의 DNS 캐시 등

**2-2. 재귀적 DNS 쿼리 시작**: 캐시에 해당 도메인이 없다면, 브라우저는 재귀적 DNS 쿼리를 시작하여 사용자에게 가까운 DNS 리졸버(일반적으로 ISP에서 제공)를 요청

**3\. 루트 DNS 서버 쿼리**: 리졸버는 루트 DNS 서버에 도메인의 최상위 도메인(TLD) 서버 정보를 요청, 루트 DNS 서버는 TLD DNS 서버(.com)의 주소를 리졸버에 반환

-   예를 들어, www.example.com의 경우 .com TLD 서버의 주소를 요청

**4\. TLD DNS 서버 쿼리**: 리졸버는 TLD DNS 서버에 요청을 전송, TLD DNS 서버는 도메인의 네임서버(예: ns1.example.com)의 IP 주소를 리졸버에 반환

**5\. 권한 있는 네임서버(Authoritative Name Server) 쿼리**: 리졸버는 해당 도메인의 권한 있는 네임서버에 최종 IP 주소를 요청

**6~7. IP 주소 반환**: 권한 있는 네임서버는 도메인의 최종 IP 주소를 리졸버에 반환하고, 리졸버는 이를 브라우저에 전달

**\+ 캐싱**: 리졸버와 브라우저는 미래의 요청 속도를 높이기 위해 이 DNS 정보를 캐시