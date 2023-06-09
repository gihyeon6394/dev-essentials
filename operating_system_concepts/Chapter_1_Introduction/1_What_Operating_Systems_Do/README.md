# 1. What Operating Systems Do

1. User View
2. System View
3. Defining Operating Systems

#### 컴퓨터 시스템의 4조각

<img src="img.png"  width="40%"/>

- 사용자
- 어플리케이션 프로그램
- OS
- 하드웨어


#### 어플리케이션 프로그램

- e.g. 워드 프로세서, 스프레드시트, 웹 브라우저 등
- 사용자에게 컴퓨팅 환경 제공

#### 하드웨어

- basic computing resources
- CPU
- memory
- I/O devices

---

OS의 역할을 완전히 이해하기 위해, 사용자 관점과 시스템 관점에서 OS를 바라보자.

## 1.1. User View

OS에 대한 사용자 관점은 사용중인 인터페이스에 따라 다르다.

#### 사용자가 1명일 때

- 최대 목적은 사용자의 퍼포먼스 최대화
- 사용자의 편의성이 우선순위
- 리소스 사용률은 신경쓰지 않음
- 모바일, 스마트 기기의 보편화
    - PC 대체제
    - 터치 스크린이나 목소리 <sup>Siri</sup>으로 시스템과 상호작용
- 사용자 관점이 적은 컴퓨터
    - 사용자 개입이 적을 것이라고 설계됨
    - e.g. 임베디드 컴퓨터, IOT 홈 기기, 자동차 스마트 기기

## 1.2. System view

- 리소스 할당자 : 시스템 관점에서 OS는 하드웨어와 밀접하게 관련된 소프트웨어
- 컴퓨터 시스템은 문제해결을 위한 다양한 리소스 가짐
    - CPU time, memory 공간, 저장 공간, I/O 기기 등
- OS는 리소스의 매니저 역할
- 리소스간에 충돌이 일어날 때 OS가 적절히 해결하여
- 사용자가 프로그램을 효과적이고 공평하게 이용하게 함
- OS는 제어 프로그램
    - 사용자 프로그램이 에러와 부적절한 사용을 방지
    - I/O 기기와 명령에 연관

## 1.3. Defining Operating Systems

- 컴퓨터 시스템은 작은 역사에도 불구 빠르게 진화해옴
- 컴퓨터 시스템의 기본적인 목적
    - 프로그램 실행
    - 사용자의 문제를 쉽게 해결

#### OS의 등장 

OS = 제어 기능 + 리소스 할당  

컴퓨터 시스템이진화함에 따라, 제어 기능과 리소스 할당기능이 하나의 소프트웨어에 합쳐졌다. 

#### OS의 경계는 모호하다

- 간단한 관점 : 운영체제 구입 시 따라오는 것
    - 시스템에 따라 운영체제가 상당히 다름
    - 크기가 각자 다름
        - 시스템에 따라 MB에서 GB까지 다름
        - 편집기, 그래픽 시스템 등

#### OS의 보편적 정의 : 커널에서 동작하는 프로그램

- **가장 보편적인 정의**
- Kernel이라는 컴퓨터에서 동작하는 프로그램
- 커널 관점에서 프로그램은 2가지 타입으로 나뉨
    - 시스템 프로그램 : OS와 연결되어있지만, 커널의 일부는 아님
    - 어플리케이션 프로그램 : OS와 연관되어있지 않은 모든 프로그램
- 모바일 OS는 핵심 커널 뿐 아니라 미들웨어도 포함함
    - 미들웨어 : 데이터베이스, 멀티미디어, 그래픽 지원
