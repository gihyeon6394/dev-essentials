# 1. Background

1. Basic Hardware
2. Address Binding
3. Logical vs Physical Address Space
4. Dynamic Loading
5. Dynamic Linking and Shared Libraries

---

### instruction-execution cycle

- 메모리는 byte 배열로 구성 각 요소는 고유의 주소를 가짐
- CPU 는 program counter 값에 따라 메모리로부터 명령 fetch
- cycle
    1. 메모리로부터 instruction fetch
    2. instruction decode
    3. operand fetch
    4. instruction execute
    5. 결과를 메모리에 저장

## 1. Basic Hardware

- main memory, register : CPU가 직접 접근 가능한 저장소
- 실행 중인 명령어, 데이터는 main memory, register에 있어야함
    - 없으면, 실행 전에 main memory에 올려야함

### Register, Main mememory, Cache

- 각 CPU core에 내장
- CPU clock cycle 마다 접근 가능
- 몇 CPU는 register 명령어를 해석, 간단한 작업 실행을 clock 한 tick 당 함
    - main memory 에서는 불가능
- 메모리 액세스 완료에는 CPU clock 여러 cycle 이 필요
    - **stall** 필요
- **cache** : register와 main memory 사이에 위치
    - 빠른 메모리 액세스를 위해

### memory space 보호

<img src="img.png"  width="60%"/>

<img src="img_1.png"  width="60%"/>

- 올바른 연산을 위해 다른 user process로부터 OS 보호 필요
- 하드웨어 단에서 보호 구현
- 보호 방법 1. : 각 process가 분리된 메모리 공간을 가짐
    - 1개 이상의 프로세스를 **동시에** 메모리에 load 할 수 있게함
    - 각 process가 접근 가능한 메모리의 범위를 정해야함
    - base register, limit register 사용
        - base register : 가장 작은 접근 가능 물리 메모리주소를 가짐
        - limit register : 범위의 크기 지정
        - e.g. base register = 300040, limit register 120900,
            - 프로그램은 300040 ~ 420940 사이의 메모리만 접근 가능
- 보호 방법 2 : CPU 하드웨어가 user mode에서 실행되는 모든 프로그램의 주소를 register와 비교
    - OS 메모리나 다른 user의 memory에 접근하려는 user-mode의 모든 시도는 OS가 error로 처리

### base, limit register

- 오직 OS만 load 가능
- base, limit register load 명령어 : special privileged instruction
    - kernel mode 에서만 실행 가능
    - OS 만이 kernel mode에서 실행 가능
    - OS는 base, limit register 값 변경 가능
    - 다른 user process는 base, limit register 값 변경 불가

### OS는 kernel mode에서 OS 메모리, user 메모리에 접근 가능

- OS는 user 프로그램을 user memory에 load
- error 가 발생한 프로그램 dump out
- system call 파라미터 수정
- user memory에 I/O 가능
- 다른 service 제공
    - e.g. multiprocessing system에서 context switch
        1. 현재 process의 context를 register에서 main memory로 저장
        2. 다음 process의 context를 main memory에서 register로 load

## 2. Address Binding

## 3. Logical vs Physical Address Space

## 4. Dynamic Loading

## 5. Dynamic Linking and Shared Libraries
