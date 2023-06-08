# 5. Resource Management

1. [process Management](#1-process-management)
2. Memory Management
3. File-System Management
4. Mass-Storage Management
5. Cache Management
6. I/O System Management

OS는 아래와 같은 리소스의 관리자이다.  
리소스는 CPU, 메모리, I/O 장치, 파일, 캐시, 저장장치 등이 있다.

## 1. Process Management

### prcoess : program in execution

- CPU에 실행되지 않으면 프로그램은 어떤 명령도 실행할 수 없음
- 프로세스 : 실행 상태의 프로그램
    - ex. 컴파일러, 워드 프로세스, SNS 앱
- 프로세스가 서브 프로세스를 동시에 실행 가능
- 프로세스는 실행 중 특정 리소스가 필요
- 실행 중인 프로세스에게 리소스를 할당
    - 리소스는 시스템 자원 뿐 아니라, 데이터 입력일 수도 있음
        - ex. 웹 브라우저 프로세스의 URL input
- 사용자 프로세스, 시스템 프로세스로 나뉨

### Program Counter <sup>PC</sup>

- program counter : 다음 실행할 명령을 가리킴
- 싱글 스레드 프로세스
    - 하나의 PC를 가짐
    - 명령 실행이 순차적
    - 프로세스 안에서 동시에 최대 하나의 명령 실행 가능
    - 하나의 프로그램에 2가지의 프로세스가 연관되어 있어도 별개로 간주, 동시 실행 불가
- 멀티 스레드 프로세스
    - 2개 이상의 PC
    - 각 PC가 각 스레드에 위치하여 다음 실행 명령 줄을 가리킴
- 프로세스 동시 실행
    - 싱글 코어 : 다중화
    - 멀티 코어 : 병렬화

### process 에 대한 OS의 책임

- 사용자/시스템 프로세스를 생성, 제거
- 프로세스와 스레드를 CPU에 스케쥴링
- 프로세스의 실행 / 중지
- 프로세스 동기화 메커니즘 제공
- 프로세스 간 통신 메커니즘 제공

## 2. Memory Management


## 3. File-System Management

## 4. Mass-Storage Management

## 5. Cache Management

## 6. I/O System Management
