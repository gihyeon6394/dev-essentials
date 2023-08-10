# 10. Summary

- race condition, 경합 상태 : 두 개 이상의 프로세스가 공유 데이터에 접근하려고 할 때, 실행 결과가 접근 순서에 의존하는 상황
- critical section, 임계영역 : 공유 데이터에 접근하여 race condition이 일어날 가능성이 있는 코드 영역
- ciritical section 해결방안
    1. mutual exclusion, 상호배제 : 동시에 하나의 프로세스만이 임계영역에 진입
    2. progress, 진행 : 프로그램은 다음에 임계영역에 진입할 프로세스를 결정해야 함
    3. bounded waiting, 유한대기 : 프로세스가 임계영역에 진입하기 위해 무한정 기다리지 않아야 함
- software solution : Peterson's solution
    - 현대 컴퓨터 구조에 부적합
- hardware solution : memory barriers, hardware instructions, CAS, atomic variables
- mutex lock : mutual exclusion 제공
    - 임계영역 진입 전 lock 요청
    - 임계영역 탈출 후 lock 반환
    - lock : binary value
- semaphore : mutual exclusion 제공
    - 임계영역 진입 전 semaphore 요청
    - 임계영역 탈출 후 semaphore 반환
    - semaphore : integer value
- monitor : high-level 의 process synchronization을 제공하는 ADT
    - condition variables : 특정 process의 condtion이 true가 되길 기다림
        - true가 되면 `signal`을 통해 다른 process에게 알림
- livness : 프로세스가 진행되는 것을 보장
    - 임계영역 문제의 해결법이 livness를 보장하지 못할 수 있음
    - e.g. deadlock
- 경합 정도에 따라 해결법의 효율이 다름
