# 9. Summary

- Deadlock : process 집합에서 모든 porcess가 다른 process에 의해 발생하는 event를 기다리는 상황
- deadlock 발생 필요 조건 4가지
    - mutual exclusion (상호 배제) : process가 필요로 하는 자원에 대해 배타적으로 접근
    - hold and wait (보유 및 대기) : process가 자원을 가지고 있으면서 다른 자원을 기다림
    - no preemption (비선점) : process가 자원을 강제로 빼앗을 수 없음
    - circular wait (순환 대기) : process 집합에서 각 process는 다음 process가 요구하는 자원을 가지고 있음
- resource-allocation graph : deadlock cycle 시각화 방법
- Deadlock 방지 : 발생 필요조건 4가지 중 1가지를 예방
    - circular wait을 회피하는 것이 가장 현실적
- banker's 알고리즘을 통한 Deadlock 회피
    - resource를 할당했을 때, safe state일떄만 할당
    - unsafe state : deadlock이 발생할 수 있는 상태
- Deadlock 복구 방법
    - circular wait의 process 중 하나를 중지
    - deadlock이 발생한 process가 가진 resource 선점
