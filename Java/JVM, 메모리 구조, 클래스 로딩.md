# [Java] JVM, 메모리 구조, 클래스 로딩

# JVM

JVM이란 자바 가상 머신의 약자로, 컴퓨터가 자바 바이트 코드를 운영체제에 맞게 실행하도록 하는 역할을 수행합니다. OS의 종류에 상관없이 자바 파일을 실행할 수 있도록 해주기 때문에 자바 언어가 플랫폼 독립적인 특성을 가집니다.

### JVM 역할

- 바이트 코드 해석 및 실행
- 메모리 관리 및 가비지 컬렉션(Garbage Collection)
- 예외 처리
- 다중 스레드 관리
- 클래스 로딩과 동적 클래스 생성

## **JVM 동작과정**

![1](https://github.com/user-attachments/assets/0442dbd2-b737-4a44-a765-aa0e1bf367eb)

1. 자바 프로그램을 실행하면 JVM은 OS로부터 메모리를 할당받는다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트 코드(.class)로 컴파일 한다.
3. **Class Loader**는 동적 로딩을 통해 필요한 클래스들을 로딩 및 링크 하여 **Runtime Data Area(실질적인 메모리를 할당 받아 관리하는 영역)**에 올린다.
4. **Runtime Data Area**에 로딩 된 바이트 코드는 **Execution Engine**을 통해 해석된다.
5. 이 과정에서 **Execution Engine**에 의해 **Garbage Collector**의 작동과 **Thread** 동기화가 이루어진다.

## **JVM 메모리 구조**

![2](https://github.com/user-attachments/assets/733b8842-63fb-4f0f-ac34-3fe1a44ef25c)

- 클래스 로더(Class Loader)
- 실행 엔진(Execution Engine)
    - 인터프리터(Interpreter)
    - JIT 컴파일러(Just-in-Time)
    - 가비지 콜렉터(Garbage collector)
- 런타임 데이터 영역 (Runtime Data Area)
    - 메소드 영역
    - 힙 영역
    - PC Register
    - 스택 영역
    - 네이티브 메소드
- JNI - 네이티브 메소드 인터페이스 (Native Medthod Interface)
- 네이티브 메소드 라이브러리 (Native Method Library)

### Runtime Data Area(런타인 데이터 영역): JVM이 OS로부터 할당받은 메모리 공간

![3](https://github.com/user-attachments/assets/370fcbc3-59f0-47d6-b16b-3565e1bf5843)

**JVM의 메모리 영역**으로 **자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재**하는 영역

- 1) 정적 메서드 영역: 스레드 간 공유되는 메모리 영역으로, 클래스/인터페이스/필드/메소드/static변수 등 바이트 코드를 보관
    - 클래스가 로딩될 때 생성
    - 클래스, 변수, 메소드 정보
    - static 변수
    - Constant pool - 문자 상수, 타입, 필드, 객체참조가 저장됨
- 2) Heap 영역: 스레드 간 공유되는 메모리 영역으로, 클래스의 인스턴스 및 배열에 대한 메모리가 할당되는 영역, 또한 가비지 컬렉션의 대상이 되는 공간
    - 런타임시 할당됨
    - new 키워드로 생성되는 객체와 배열이 저장되는 영역
- 3) JVM Stack 영역: 각 스레드 별로 생성되는 메모리 영역으로, 로컬변수와 메서드 호출 정보 등을 저장
    - 컴파일 타임시 할당
    - Heap 영역에 생성되는 객체의 주소 값을 가짐
    - 힙과 스택은 같은 메모리 공간을 동적으로 공유하며, 과도하게 사용하는 경우 OOM(Out Of Memory)이 발생
- 4) PC 레지스터: 현재 스레드의 실행 주소가 담기는 영역
    - 스레드가 생성될 때마다 생성되는 영역으로 다음 명령어의 주소를 저장
- 5) 네이티브 메서드 스택: Java 코드가 아닌 다른 언어로 된 코드를 실행할 때 사용되는 메모리 영역

### 메서드 영역(Method Area) 혹은 Class Area, Static Area

**메서드 영역**은 JVM이 시작될 때 생성되는 공간으로 **바이트 코드(.class)**를 처음 메모리 공간에 올릴 때 **초기화되는 대상을 저장**하기 위한 메모리 공간이다.

JVM이 동작하고 클래스가 로드될 때 적재되서 **프로그램이 종료될 때까지 저장** 된다.

![4](https://github.com/user-attachments/assets/f0d55275-a604-4fb5-aa2e-8a6e34255f38)

모든 쓰레드가 공유하는 영역이라 다음과 같이 초기화 코드 정보들이 저장되게 된다.

- Field Info : 멤버 변수의 이름, 데이터 타입, 접근 제어자의 정보
- Method Info : 메소드 이름, return 타입, 함수 매개변수, 접근 제어자의 정보
- Type Info : Class 인지 Interface 인지 여부 저장, Type의 속성, 이름 Super Class의 이름

### **힙 영역 (Heap Area)**

![5](https://github.com/user-attachments/assets/b0914e04-474e-4ab4-a101-3d020876e6e1)

**힙 영역**은 메서드 영역와 함께 모든 쓰레드가 공유하며, JVM이 관리하는 프로그램 상에서 데이터를 저장하기 위해 런타임 시 동적으로 할당하여 사용하는 영역이다.

즉, **new 연산자로 생성되는 클래스와 인스턴스 변수, 배열 타입 등 Reference Type이 저장**되는 곳이다.

당연히 Method Area 영역에 저장된 클래스만이 생성이 되어 적재된다.

유의할점은 힙 영역에 생성된 객체와 배열은 **Reference Type**으로서, JVM 스택 영역의 변수나 다른 객체의 필드에서 참조된다는 점이다.

즉, 힙의 참조 주소는 "스택"이 갖고 있고 해당 객체를 통해서만 힙 영역에 있는 인스턴스를 핸들링할 수 있는 것이다.

힙의 객체는 스택의 참조 타입 변수와 연결되어 있다

![https://blog.kakaocdn.net/dn/qEBMZ/btrINtTDQbX/MJ2ODVUaLoVPvauKUBXsgK/img.png](https://blog.kakaocdn.net/dn/qEBMZ/btrINtTDQbX/MJ2ODVUaLoVPvauKUBXsgK/img.png)

만일 참조하는 변수나 필드가 없다면 의미 없는 객체가 되기 때문에 이것을 쓰레기로 취급하고 JVM은 쓰레기 수집기인 **Garbage Collector**를 실행시켜 쓰레기 객체를 힙 영역에서 자동으로 제거된다.

### 스택 영역(Stack Area)

스택 영역은 int, long, boolean 등 기본 자료형을 생성할 때 저장하는 공간으로, **임시적으로 사용되는 변수나 정보들이 저장되는 영역**이다.

![https://blog.kakaocdn.net/dn/IWMvu/btrINLlRy5q/47kyqs0O8ITVRXfXYvHS0K/img.png](https://blog.kakaocdn.net/dn/IWMvu/btrINLlRy5q/47kyqs0O8ITVRXfXYvHS0K/img.png)

자료구조 Stack은 마지막에 들어온 값이 먼저 나가는 LIFO  구조로 push와 pop 기능 사용방식으로 동작한다.

메서드 호출 시마다 각각의 **스택 프레임(그 메서드만을 위한 공간)**이 생성되고 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장한다.

그리고 메서드 수행이 끝나면 프레임별로 삭제된다.

# 클래스 로딩 과정

## 클래스 로더

![6](https://github.com/user-attachments/assets/47778ac5-9bc4-4c56-834f-0f488ec8b8ca)

- JVM 내로 클래스 파일(*.class)을 동적으로 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈
- 즉, 로드된 **바이트 코드(.class)들을 엮어서 JVM의 메모리 영역인 Runtime Data Areas에 배치**
- 클래스를 메모리에 올리는 로딩 기능은 한번에 메모리에 올리지 않고, 어플리케이션에서 필요한 경우 동적으로 메모리에 적재

## 클래스 로드 과정

### 클래스 로딩 요청이 오면

- 클래스를 올려달라는 요청을 받으면
    - 1) 해시에 해당 클래스가 있는지 확인
    - 2) 상위 클래스 로더에서 해당 클래스가 있는지 확인
    - 3) 본인 클래스 로더에서 가져옴

### 클래스 로딩

![7](https://github.com/user-attachments/assets/a60029fd-4845-4aca-bdcc-d9a691dd0995)

1. **Loading(로드)** : 클래스 파일을 가져와서 JVM의 메모리에 로드한다.
2. **Linking(링크)** : 클래스 파일을 사용하기 위해 검증하는 과정이다.
    1. **Verifying(검증)** : 읽어들인 클래스가 JVM 명세에 명시된 대로 구성되어 있는지 검사한다.
    2. **preparing(준비)** : 클래스가 필요로 하는 메모리를 할당한다.
    3. **Resolving(분석)** : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경한다.
3. **Initialization(초기화)** : 클래스 변수들을 적절한 값으로 초기화한다. ( static 필드들을 설정된 값으로 초기화 등 )

### **클래스 로더의 위계 순서**

- Bootstrap Class Loader
    - JVM 실행 시 가장 먼저 실행됨
    - 자바의 기본적인 클래스 로드
- Extension CL
    - 자바 기본 클래스 제외한 확장 클래스 로드
- System CL
    - 자바 실행 시 classpath 에 정의된 클래스들 로드
- User Defined CL
    - 유저가 직접 클래스 로더를 정의하여 사용할 수 있음
