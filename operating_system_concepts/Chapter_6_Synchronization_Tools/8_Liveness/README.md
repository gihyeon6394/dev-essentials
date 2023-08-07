# 8. Liveness

1. Deadlock
2. Priority Inversion

---

indefinite waiting  : progress와 bounded-waiting criteria를 만족시키지 못함

### Liveness

- process가 실행주기 동안 만족해야하는 진행 속성
- **Liveness failure** : Livness을 만족시키지 못하는 상황
- 주요 원인 : 부족한 성능, 응답 속도
- e.g. 무한 loop, mutex lock, semaphore
- Livness가 발생하는 경우
    - deadlock
    - priority inversion

## 1. Deadlock

| time | P0           | P1           |
|------|--------------|--------------|
| T0   | `wait(S);`   | `wait(Q);`   |
| T1   | `wait(Q);`   | `wait(S);`   |
| ...  | ...          | ...          |
| Tn   | `signal(S);` | `signal(Q);` |
| Tn+1 | `signal(Q);` | `signal(S);` |

- T1 : P0이 Q를 얻기 위해 대기, P1이 S를 얻기 위해 대기
    - P0은 `signal(Q);`가 실행되기 전까지 block
    - P1은 `signal(S);`가 실행되기 전까지 block

### Deadlock?

- 임의의 집합 안의 모든 process가 집합 안의 다른 프로세스의 특정 **이벤트**를 기다리는 상황
    - 이벤트 : 자원의 할당 or 해제 (mutex lock, semaphore, ...)

## 2. Priority Inversion

- 보통 2가지 이상의 우선순위가 존재할 때 발생
    - 더 높은 우선순위의 process가 더 낮은 순위의 process가 가지고 있는 kernel data에 접근하려할 때
    - kernel data 에 lock을 획득하기위해 기다려야함 (일반적으로)
    - 이 떄 더 낮은 프로세스가 더 높은 또 다른 프로세스에 선점당하면,
    - **우선순위가 역전됨**

### 예시

- Process L, M, H
- 우선순위 : L < M < H
- L이 semaphore S를 사용중
- H가 S를 요청 및 대기
- M이 **runnable** 상태로 전환 & **L을 선점**
    - H는 M보다 높은 우선순위를 가지지만, S를 더 먼저 얻음

### 해결책 : priority-inheritance protocol

- 더 높은 우선순위 프로세스가 필요로하는 리소스에 접근 중인 더 낮은 프로세스가 있을 때,
- 더 낮은 프로세스는 더 높은 프로세스의 우선순위를 상속받음
- 다른 프로세스가 선점하려해도 하지 못함
- 더 높은 프로세스가 리소스를 해제하면, 더 낮은 프로세스는 원래 우선순위로 복귀
- 기존에 wait중인 더 높은 우선순위 프로세스는 리소스 할당 받음
