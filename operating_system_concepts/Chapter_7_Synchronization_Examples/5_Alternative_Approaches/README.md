# 5. Alternative Approaches

1. Transactional Memory
2. OpenMP
3. Functional Programming Languages

---

## 1. Transactional Memory

- memory transaction : atomic operation의 집합
- transaction의 모든 atomic 연산이 완료되면 commit
- 연산은 모두 성공하거나, 모두 실패해야 함
- **Software Transactional Memory (STM)**
    - 소프트웨어 수준에서 트랜잭션을 구현
- **Hardware Transactional Memory (HTM)**
    - 하드웨어 수준에서 트랜잭션을 구현
    - 분리된 프로세서 간의 공유 data의 충돌을 해결하기 위함

## 2. OpenMP

````
voud update(int value) {
    #pragma omp critical
    {
        count += value;
    }
}

````

- `#pragma omp` : OpenMP directive
- OpenMP 라이브러리에 의해 스레드가 생성되고 관리되는 장점
- `#pragma omp critical` : critical section을 정의
    - 임계영역에 하나의 thread만 진입
- deadlock 발생 가능성

## 3. Functional Programming Languages
