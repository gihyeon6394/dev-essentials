# 3. System Calls

1. Example
2. Application Programming Interface
3. Types of System Calls

---

- System Call은 OS에서 제공하는 서비스에 대한 인터페이스를 제공
- C, C++ 로된 함수 형태로 제공
- 하드웨어에 직접 접근 작업의 경우 어셈블리어 명령어 작성 필요

## 1. Example

#### 파일 복사

```shell
cp in.txt out.txt
```

<img src="img.png"  width="50%"/>

1. 프로그램 입력에 input 파일, output 파일 지정
    - UNIX : 파일명, 와일드 카드 등의 형태로 지정 가능
    - 대화형 OS :  프로그램이 사용자에게 파일명 물어봄
    - 마우스/키보드 기반 OS : 파일 목록을 디스플레이로 표시, 사용자가 입력장치로 선택, 많은 I/O 발생
2. 파일 세팅 :  프로그램이 input 파일을 열고 <sup>1</sup>, output 파일을 만들고 엶 <sup>2</sup>
    - 각자 system call 발생하고, 각자 에러 핸들링 필요
        - input 파일 : 파일이 없거나, 사용자가 읽기 권한이 없거나 등, 비정상 종료 시킴
        - output 파일 : 파일이 이미 있으면 삭제 후 생성 등
3. 파일 복사 : input을 읽고 <sup>system call</sup> output에 쓰기<sup>system call</sup> 반복
    - 각 작업에서 상태 정보를 반환해야함
    - 읽기 : 파일의 끝에 도달했는지, 파일의 끝에 도달했다면 EOF 반환 등
    - 쓰기 : 디스크가 가득 찼는지, 가득 찼다면 에러 반환 등
4. 복사 완료 : 두 파일을 닫고,메시지를 콘솔에 출력

## 2. Application Programming Interface <sup>API</sup>

- 간단한 프로그램 실행에 많은 system call이 필요
- 그 내용을 알지 않고 API를 통해 사용
    - API : fntion, 매개변수, 결과 기대값 들의 집합 <sub>ex. POSIX API, Windows API, Java API 등</sub>
- libc : C 언어로 작성된 Unix, Linux, macOS의 API
- API의 함수들이 system call을 호출 <sub>프로그래머 대신</sup>
    - ex. Window funciton CreateProcess()는 Window Kernel의 system call NTCreateProcess()를 호출

#### API를 통해 system call을 호출하는 이유

- portability <sup>이식성</sup> : API를 지원하는 시스템이 바뀌어도, 컴파일된 API가 실행 가능
- 복잡성 : System call이 API보다 복잡하고 자세함

#### run-time environment <sup>RTE, 실행환경</sup> 과 system-call interface

<img src="img_1.png"  width="50%"/>

- RTE : 응용프로그램이 실행되는 환경
    - 실행에 필요한 소프트웨어, 컴파일러, 라이브러리, 로더 등 포함
- RTE는 System call에 대한 인터페이스 역할
    - API 가 호출한 System call을 가로채고, OS의 system call을 호출
- 이 인터페이스는 각 System call에 번호 할당하여 번호를 인덱스로 테이블 유지
- API 사용자는 System call 내부 구현을 모름
- System call 내용을 숨기고, RTE가 관리

#### OS에 파라미터를 보내는 방법

<img src="img_2.png"  width="50%"/>

- 레지스터
- 블록, 테이블, 메모리 : 레지스터 수보다 파라미터가 많을 때
- Linux는 두 방법을 혼용
    - 5개 정도의 파라미터면 레지스터, 아니면 블록 메서드
- 블록메서드 외에도 stack을 사용
    - stack이나 블록은 파라미터수 제한이 없는 장점

## 3. Types of System Calls

### 3.1 Process Control

### 3.2 File Management

### 3.3 Device Management

### 3.4 Information Maintenance

### 3.5 Communication

### 3.6 Protection

