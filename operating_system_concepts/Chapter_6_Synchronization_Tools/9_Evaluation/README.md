# 9. Evaluation

### hardware solution  CAS (compare-and-swap)

- low level
- mutex lock 과 같은 동기화 도구를 구현하는 것의 기초로 사용
- CAS 는 **lock-free** synchronization 을 구현하는데 사용됨
    - race codition을 피하고, lock overhead가 없음
    - 개발과 테스트가 어려움

### CAS vs Mutex lock

|                                 | Mutex lock                     | CAS                     |
|---------------------------------|--------------------------------|-------------------------|
| 접근 방법                           | pessimistic(비관적)               | optimistic<br/>(낙관적)    |
| uncontended<br/>(경합이 없는)        | 느림                             | 더 빠름                    |
| moderate contention<br/>(중간 경합) | 느림<br/>lock 획득 코드가 복잡하고 시간이 소요 | 더 빠름<br/>획득하기 전까지 몇번 반복 |
| high contention<br/>(높은 경합)     | 더 빠름                           | 느림                      |

#### moderate contention

- Mutex lock : lock 획득하기 전까지 시간 소요, 코드 복잡
    - 스레드 중단, waiting queue 에 enqueue, context switch 등 작업 수반
- CAS : 대부분 Lock을 획득, 실패해도 몇번만 반복 시도

### mechanism

- atomic integer가 전통적인 lock (mutex lock, semaphore) 보다 가벼움
    - e.g. multiprocessor 환경의 spinlock
- mutex lock vs semaphore
    - mutex lock이 semaphore 보다 더 간단하고 효율적
    - 유한한 리소스에 접근할 떄는 semaphore 가 적합

### high-level synchronization tools : monitor, condition variable

- 간단하고 사용이 편함
- 구현에 따라 상당한 오버헤드와 race condition 발생 가능
- 해결방안
    - 컴파일러가 더 효율적인 코드를 생성하도록 설계
    - concurrent programming을 지원하는 언어로 개발
    - 라이브러리와 API의 성능 향상
