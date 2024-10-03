# [CS스터디] 아자자04

## 📕 스터디 개요

- 주제: 백엔드 Java 개발자 면접을 위한 CS 스터디
- 일정: 대략 3개월(2024.08.19 ~)
- 모임: 주2회(온/오프 병행)
    - 월요일 - 온라인(디스코드) 9시
    - 목요일 - 오프라인(건대입구) 8시

## 👨‍🏫 진행 방식

- 1차와 2차를 구분하여 진행
    - 1차: CS 공부(주마다 주제를 정하고 세부 분야를 나누어 각자 조사 + 발표, 질문, 토론)
    - 2차: 모의면접(꼬리질문으로 묻고 답하기)
- 발표내용은 **.md** 파일로 정리해서 GitHub 레포에 push, 개인브랜치에서 작업 후 목요일마다 merge
- 커밋메시지: **[주제] 소주제**
    - ex) [Operating System] 프로세스 vs 스레드

## 📋 커리큘럼

- **운영체제 (Operating System)**
  - [운영체제란](/Operating%20System/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C%EB%9E%80.md)
  - [프로세스와 스레드](/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%8A%A4%EB%A0%88%EB%93%9C.md)
  - [프로세스 주소 공간 & 프로세스 간 통신(IPC)](/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%20%EC%A3%BC%EC%86%8C%20%EA%B3%B5%EA%B0%84%20%26%20%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%20%EA%B0%84%20%ED%86%B5%EC%8B%A0%20(IPC).md)
  - [CPU 스케줄링](/Operating%20System/CPU%20%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81.md)
  - [데드락](/Operating%20System/%EB%8D%B0%EB%93%9C%EB%9D%BD.md)
  - [세마포어와 뮤텍스](/Operating%20System/%EC%84%B8%EB%A7%88%ED%8F%AC%EC%96%B4%EC%99%80%EB%AE%A4%ED%85%8D%EC%8A%A4.md)
  - [인터럽트 & 시스템 콜](/Operating%20System/%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8%20%26%20%EC%8B%9C%EC%8A%A4%ED%85%9C%20%EC%BD%9C.md)

- **네트워크 (Network)**
  - [DNS와 웹 통신 흐름](/Network/DNS%EC%99%80%20%EC%9B%B9%20%ED%86%B5%EC%8B%A0%20%ED%9D%90%EB%A6%84.md)
  - [OSI 7계층, TCP/IP 4계층 & L4, L7 스위치와 로드밸런싱](/Network/OSI%207%EA%B3%84%EC%B8%B5%2C%20L4L7%EC%8A%A4%EC%9C%84%EC%B9%98%2C%20%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1.md)
  - [REST API & 쿠키와 세션 & SOP, CORS](/Network/REST%20API%20%26%20%EC%BF%A0%ED%82%A4%EC%99%80%20%EC%84%B8%EC%85%98%20%26%20SOP%2C%20CORS.md)
  - [TCP IP 흐름제어 및 혼잡제어](/Network/TCP%20IP%20%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20%EB%B0%8F%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.md)
  - [TDP & UDP, Handshake](/Network/TDP%20%26%20UDP%2C%20Handshake.md)
  - [동기 비동기, 블록킹 넌블록킹](/Network/%EB%8F%99%EA%B8%B0%20%EB%B9%84%EB%8F%99%EA%B8%B0%2C%20%EB%B8%94%EB%A1%9D%ED%82%B9%20%EB%84%8C%EB%B8%94%EB%A1%9D%ED%82%B9.md)

- **DB (Database)**
  - [SQL injection](/Database/SQL%20Injection.md)
  - [SQL join](/Database/SQL%20Join.md)
  - [Index](/Database/index.md)
  - [트랜잭션과 트랜잭션 격리수준](/Database/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%2C%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EA%B2%A9%EB%A6%AC%EC%88%98%EC%A4%80.md)
  - [파티셔팅, 샤딩, 레플리케이션, 클러스터링](/Database/%ED%8C%8C%ED%8B%B0%EC%85%94%EB%8B%9D%2C%EC%83%A4%EB%94%A9%2C%EB%A0%88%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%2C%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EB%A7%81.md)

- **디자인패턴 (Design Pattern)**
  - [브릿지 패턴](https://github.com/azaza04/cs-study/blob/main/Design%20Pattern/%EB%B8%8C%EB%A6%BF%EC%A7%80%20%ED%8C%A8%ED%84%B4.md)
  - [옵저버 패턴](https://github.com/azaza04/cs-study/blob/main/Design%20Pattern/%EC%98%B5%EC%A0%80%EB%B2%84%20%ED%8C%A8%ED%84%B4.md)
  - [프록시 패턴](https://github.com/azaza04/cs-study/blob/main/Design%20Pattern/%ED%94%84%EB%A1%9D%EC%8B%9C%20%ED%8C%A8%ED%84%B4.md)
  - [싱글톤 패턴](https://github.com/azaza04/cs-study/blob/main/Design%20Pattern/%EC%83%9D%EC%84%B1%ED%8C%A8%ED%84%B4_%EC%8B%B1%EA%B8%80%ED%86%A4%ED%8C%A8%ED%84%B4.md)
  - [프로토타입 패턴](https://github.com/azaza04/cs-study/blob/main/Design%20Pattern/%EC%83%9D%EC%84%B1%ED%8C%A8%ED%84%B4_%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%ED%8C%A8%ED%84%B4.md)
  - [전략 패턴](https://github.com/azaza04/cs-study/blob/main/Design%20Pattern/%ED%96%89%EB%8F%99%ED%8C%A8%ED%84%B4_%EC%A0%84%EB%9E%B5%ED%8C%A8%ED%84%B4.md)
  - [템플릿 메서드 패턴](https://github.com/azaza04/cs-study/blob/main/Design%20Pattern/%ED%96%89%EB%8F%99%ED%8C%A8%ED%84%B4_%ED%85%9C%ED%94%8C%EB%A6%BF%EB%A9%94%EC%84%9C%EB%93%9C%ED%8C%A8%ED%84%B4.md)

- **자료구조 (Data Structure)**
  - [Heap](/Data%20Structure/Heap.md)
  - [Graph](/Data%20Structure/Graph.md)
  - [Tree](/Data%20Structure/%ED%8A%B8%EB%A6%AC.md)

- **알고리즘 (Algorithm)**

- **자바&스프링&코틀린 (Java & Spring & Kotlin)**
  - [GC(Garbage Collector)](/Java/GC(Garbage%20Collector).md)
  - [JVM, 메모리 구조, 클래스 로딩](/Java/JVM%2C%20%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EA%B5%AC%EC%A1%B0%2C%20%ED%81%B4%EB%9E%98%EC%8A%A4%20%EB%A1%9C%EB%94%A9.md)


