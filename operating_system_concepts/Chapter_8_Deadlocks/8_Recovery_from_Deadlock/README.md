# 8. Recovery from Deadlock

1. Process and Thread Termination
2. Resource Preemption

---

- deadlock 탐지 알고리즘에 의해 deadlock이 감지되면,
- 몇가지 recovery scheme이 가능함
- 방법 1. operator에게 deadlock을 알리고, 수동으로 처리하도록 함
- 방법 2. system이 스스로 복구
    - circular wait 에 참여한 thread를 중지하는 방법
    - deadlock 상태의 thread가 가진 resource를 선점하는 방법

## 1. Process and Thread Termination

### process, thread를 중지시켜 deadlock을 제거하는 방법

- 두 방법 모두 process에 할당된 resource를 회수
- 방법 1. deadlock process 중지
    - deadlock cycle을 완전히 없앰, 비용이 높음
    - deadlock 전까지 연산이 많았으면 나중에 다시 연산을 해야함
- 방법 2. deadlock cycle이 없어질 때까지 process 하나하나 중지
    - 하나씩 중지할 때마다 감지 알고리즘 호출 (overhead 큼)
    - 중지 비용이 낮은 process부터 하나씩 중지

#### 중지 비용의 기준 : 중지할 process의 선택

- process의 우선순위
- process의 지금까지 연산 시간과 남은 시간
- process가 사용중인 resource type 수
    - resource를 선점하기 간단한가
- process가 앞으로 요청할 resource 수
- 종료되어야하는 process 수

## 2. Resource Preemption

- resource 선점을 통해 deadlock으로부터 복구하려면,
    - resource를 선점할 수 있는 방법이 필요
    - 선점한 resource를 다른 process에게 할당해야함

### deadlock을 위한 resource 선점 방법

- **희생자 정하기** : 어떤 resource와 process를 선점할 것인가
    - 가장 작은 선점 비용을 우선순위로 선정
    - 비용 : 할당한 resource 수, 남은 연산 시간
- **Rollback** : resource를 선점하면 해당 process는 어떻게 되는가?
    - 정상 실행을 이어갈 수 없음
    - process의 작업을 safe state로 rollback시키고 해당 state에서 재시작 시켜야함
    - safe state가 애매하면 전체 rollback
- **Starvation** (기아) : resource가 항상 같은 process부터 선점당하지 않게 보장
    - cost 기반의 system에선 항상 같은 process가 희생자가 됨
        - 해당 process는 complete 불가능
    - cost에 rollback 회수를 포함시켜 계산


