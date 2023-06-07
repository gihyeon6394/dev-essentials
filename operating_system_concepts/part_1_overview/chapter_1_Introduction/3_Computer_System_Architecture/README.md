# 3. Computer-System Architecture

1. Single-Processor Systems
2. Multiprocessor Systems
3. Clustered Systems

---

### DEFINITIONS OF COMPUTER SYSTEM COMPONENTS

- CPU : 명령을 실행하는 하드웨어
- Processor : CPU 내부에서 1개 이상의 물리 칩
- Core : CPU의 기본 연산 유닛
- Multicore : 1개의 CPU <sup>processor</sup>, 2개 이상의 core
- Multiprocessor :2개 이상의 프로세서

컴퓨터 시스템을 프로세스 수에 따라 분류할 수 있다.

## 1. Single-Processor Systems

- 옛날에는 대부분의 시스템이 하나의 CPU를 가지는 단일 프로세서를 사용

#### special-purpose processors

- 디스크, 키보드, 그래픽 컨트롤러 등에 위치
- 제한된 명령 수행
- 프로세스를 구동하지 않음
- OS에 의해 관리될 수 있음
- ex.
    - 디스크 컨트롤러 마이크로프로세서 : 메인 CPU로부터 요청을 받아 자신의 디스크 큐에 실행

## 2. Multiprocessor Systems

- 전통적으로 싱글코어 CPU가 있는 프로세서가 2개 이상인 시스템
- 프로세서가 많아지면,
    - 동일 시간의 처리량이 늘어남
- N개의 프로세서에서 속도 향상 비율은 N보다 작음
    - 여러 프로세스가 협력 시 올바른 작동을 유지하도록 일정량의 오버헤드 발생

### symmetric multiprocessing <sup>SMP, 대칭형 다중 처리</sup>

<img src="img.png"  width="50%"/>  

- 다중 처리 프로세서 시스템의 가장 일반적인 형태
- 프로세서가 본인의 레지스터들을 가지지만,
- 시스템 버스의 물리적인 공유 메모리를 통해 데이터 공유
- N개의 프로세서가 N개의 일을 동시에 수행할 수 있음
- 하나의 프로세서서는 idle, 하나의 프로세서는 바쁜 상태일 수 있음
    - 프로세서 간에 프로세스, 메모리와 같은 자료구조 리소스를 동적으로 공유하여 해소
    - 주의 필요

### multicore system

<img src="img_1.png"  width="50%"/>  

- 1개의 프로세서, 멀티 코어
- 프로세서가 2개 이상인 전통적인 시스템보다 효율 높음
    - 칩 내부 커뮤니케이션이 칩-칩 커뮤니케이션보다 빠름
    - 전력 소모 덜함 <sub>모바일 디바이스 같은 곳에 특히 유효</sub>
- L1 Cache : 로컬 캐시
- L2 Cache : shared 캐시, 각 코어와 공유

#### non-uniform memory access <sup>NUMA</sup>

<img src="img_2.png"  width="40%"/>  

- CPU가 많아지면 전력소모가 많아지고, 시스템 버스에 보틀넥 발생, 성능 저하
- 그에 따른 대안으로
- 작고 빠른 로컬 버스를 통해 접근 가능한 로컬 메모리 배치
- 성능 향상 : 로컬 메모리를 통해 접근
- 시스템 전반의 성능 저하 없음
- CPU는 나의 물리적 주소 공간 공유
- 현대 고성능 서버의 인기가 많아지고 있음
- 단점
    - 레이턴시 : CPU가 다른 CPU의 메모리에 접근할 때는 시스템 버스를 통해 접근
    - OS는 CPU 스케줄링과 메모리 관리로 해소

### blade server

- 1 bay, 멀티 프로세서 보드 + IO 보드 + 네트워킹 보드
- 각 블레이드 프세서가 독립적으로 운용되고, 자신들의 OS를 가짐
- 2개 이상의 독집적인 멀티프로세서 시스템

## 3. Clustered Systems
