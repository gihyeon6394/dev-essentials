# 6. Threading Issues

1. the fork() and exec() System Calls
2. Signal Handling
3. Thread Cancellation
4. Thread-Local Storage
5. Scheduler Activations

---

mulithreaded program을 설계할 때 고려해야할 몇가지 issue

## 1. the fork() and exec() System Calls

- `fork()` 시 2가지 버전으로 일할 수 있음
    1. parent process의 thread 전체 복사해서 process 생성
        - `fork()` 후 `exec()` 바로 호출하지 않을 때 적합
    2. paretn process의 `fork()` 호출한 thread만 복사하여 single-threaded process 생성
        - `fork()` 후 바로 `exec()` 호출할 때 적합

## 2. Signal Handling

#### signal

- Unix System에서 process에게 이벤트가 발생했음을 알리는 것
- signal은 동기/비동기적으로 수신할 수 있음
- 동작순서
    1. signal 발생 : 특정 이벤트에 의해 생성됨
    2. signal 전달 : process에게 전달됨
    3. signal 처리 : process가 signal을 처리함

#### synchronous signal

- process가 잘못된 메모리 접근이나 0으로 나누기와 같은 오류를 발생시키고,
- signal을 해당 process에게 전달

#### asynchronous signal

- 외부 process에 의해 발생
    - ex. ctrl + c 로 종료

#### signal을 처리하는 handler

- default signal handler
    - 모든 signal에 대해 default handler가 존재
    - kernel이 signal을 처리할때 사용
- user-defined signal handler
    - default singal handler를 재정의
    - signal이 무시되거나, 잘못된 실행에 대해서는 process를 종료시킬 수 있음

#### multi-thread 환경에서 signal 처리하기

- single-threaded program은 signal이 생기면 항상 process에게 전달됨
- multi-threaded program은 signal이 생기면 process, thread에게 전달할 수 있음
    - signal을 적용할 thread에 전달
    - process의 모든 thread에게 전달
    - process의 특정 thread에 전달
    - process의 특정 thread를 signal 수신전용으로 할당
- synchronous signal은 항상 thread에게 전달하면 됨
- asynchronous signal은 signal에 따라 다름
    - ctrl + c : 모든 thread에게 전달해야함

#### Unix System signal

```c
// Unix System signal을 보낼 process를 정하는 함수
kill(pidt pid, int signal);
```

- `pid` : signal을 전달할 process의 pid
- `signal` : 전달할 signal
- multithreaded Unix System은 thread가 어떤 signal을 받을지 말지 판단함

```c
// POSIX Pthread signal
pthread_kill(pthread_t thread, int signal);
```

#### Window asynchronous procedure calls <sup>APC</sup>

- Window는 명시적으로 signal을 지원하지 않음
- APC를 사용하여 signal을 구현함

## 3. Thread Cancellation

## 4. Thread-Local Storage

## 5. Scheduler Activations

