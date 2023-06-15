# 6. Why Applications Are Operating-System Specific

기본적으로 한 OS에 컴파일된 어플리케이션은 다른 OS에서 실행 불가  
OS는 어플리케이션에게 OS 고유의 System call을 제공하기 때문이다.

그럼에도 불구하고 어플리케이션은 여러 OS에서 실행할 수 있는 방법을 가지고 있다.

### 어플리케이션이 여러 OS와 호환될 수 있는 방법

#### Interpreted Languages

- 인터프리터를 거쳐서 실행되는 프로그래밍 언어 <sub>ex. Python, Ruby</sub>
- 인터프리터가 프로그램 소스를 읽고 해당하는 기계어로 변환하여 실행
- 성능은 다소 떨어짐

#### Virtual Machines 에서 실행되는 언어

- Virtual Machine : 언어의 Full Runtime Environment
- 언어가 Virtual Machine 위에서 실행되기 때문에, OS에 독립적
- Java의 Java Virtual Machine <sub>JVM</sub>이 대표적
    - loader, 바이트코드 검증기 등의 컴포넌트를 가지고 JVM에 java application을 로드하고 실행
    - 이론적으로 Java 앱이 RTE 내에서 어느 OS에건 실행 가능

#### Standard language or API

- OS에서 실행가능한 이진파일을 생성하는 표준 언어나 API
- ex. POSIX API <sup>Portable Operating System Interface</sup>
    - 서로 다른 UNIX OS 간의 공통 API를 정의
    - 이식성이 높은 UNIX 어플리케이션을 개발하기 위함

---

- 응용프로그램 수준에서 OS를 다루는 라이브러리를 제공함
- GUI 형태로 제공하는 OS 라이브러리는 OS에 의존적임
    - ex. IOS 기반의 아이폰 응옹프로그램은 Android 기반에서 실행 불가

#### OS별로 다른 것들

- 실행 파일 내부 구조
    - 헤더, 명령어, 변수 레이아웃 지정 등
- CPU의 명령어 집합
- System call
    - 피연산자, 피연산자 순서, 호출하는 방법 등

#### 해소 방안

- Linux의 ELF <sup>Executable and Linkable Format</sup> 포맷의 바이너리 실행 파일
- ABI <sup>application binary interface</sup> : 아키텍쳐 레벨에서 통일한 API
    - 바이너리 코드가 다른 구성요소와 상호방식하는 방법을 정의
        - ex. 주소 너비, System call에 파라미터 전달 방법, 시스템 라이브러리 이진 형식 등
    - 실행 파일이 컴파일되고 특정 ABI에 따라 link되면, 해당 ABI를 지원하는 OS라면 어디든 실행 가능

  



