# 8. Summary

- thread는 CPU의 기본 활용 단위이며, process에 속한 thread는 process의 자원을 공유함
- multithreaded application의 장점
    1. 응답성
    2. 자원 공유
    3. 경제적
    4. 확장성
- Concurrency는 mult-threaded 환경에서 일어난다. Parallelism은 mult-threaded 환경에서 동시에 진행할 때 일어난다.
    - Sincle CPU에서 Concurrency 가능
    - Multiple CPU에서 Parallelism 가능
- multithreaded apllication 설계의 주요 고려사항
    - 일을 나누고 균형을 맞춤
    - 데이터를 각 thread에게 분배하고 데이터의 의존성 식별
    - 프로그램의 테스트와 디버깅
- Data parallelism은 데이터를 각 Core에게 분배하고, 각 core에서 동일한 실행을 수행하는 것
    - Task parallelism은 각 Core에게 서로 다른 Task를 할당하고, 각 Core에서 서로 다른 실행을 수행하는 것
- user-level thread
    - user-level thread : User applicataion이 생성하여 추후 kernel thread에 매핑되어 CPU에서 실행됨
    - many-to-one model, one-to-one model, many-to-many model
- thread library : thread를 생성하고 관리하는 API 제공
    - Windows : Windows System
    - Pthreads : POSIX 호환 시스템, Unix, Linux, macOS 등
    - Java threads : JVM 지원되는 시스템
- Implicit threading은 API나 Language가 task를 식별하여 thread를 생성하고 관리하게 하는 것
    - thread pool, fork-join framework, Grand Central Dispatch
    - concurrency, parallel application에서 필요성이 증가함
    - Thread는 asynchronous 혹은 deffered cancellation에 의해 종료 가능. deffered 더 선호
        - asynchronous : threa를 즉시 중지
        - deffered : thread가 정상 종료가 가능할 때까지 대기하다 종료
- Linux는 Process와 thread를 구분하지 않고 Task로 구분함
    - `clone()` system call : thread나 process와 같은 task를 생성함
