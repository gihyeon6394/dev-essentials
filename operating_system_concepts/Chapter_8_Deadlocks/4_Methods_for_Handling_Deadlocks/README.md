# 4. Methods for Handling Deadlocks

### Deadlock을 다루는 방법

1. 시스템에서 deadlock 발생 가능성이 없다고 간주하고, 무시
    - 가장 비용이 저렴
    - 대부분의 OS에서 사용 (e.g. Linux, Windows)
    - kernel, application developer가 deadlock을 피하기 위해 프로그램을 작성해야함
2. deadlock state에 빠질 가능성이 전혀 없는 protocol을 사용
    - OS developer들이 사용
3. deadlock -> 감지 -> 복구 : deadlock이 발생하면, system이 deadlock을 감지하고 복구
    - Database
4. Livelock : deadlock과 유사하지만, thread가 계속해서 state를 변경하려고 시도
    - deadlock이 아님

### Deadlock prevention vs Deadlock avoidance

- Deadlock prevention : deadlock이 발생하지 않도록 **방지**
- Deadlock avoidance : deadlock이 발생하지 않도록 **회피**
    - OS는 thread가 사용할 리소스 정보를 추가로 받아야함
    - 사전정보를 사용해서 thread의 wait 여부를 결정

### Deadlock method가 없으면,

- 탐지되지 못한 deadlock이 시스템 성능 저하시킴
- resource가 thread에 의해 계속 점유됨
- system 기능이 멈추고 수동으로 재시작해야함
