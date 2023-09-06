# 1. System Model

### resource type, instance

- 유한한 resource로 구성된 system은 몇개의 경쟁하는 thread들한테 분배
    - 유한한 resource : CPU cycles, files, I/O device, semaphores, mutex locks
- resource 유형의 개수만큼 instance가 존재
    - e.g. CPU가 4개면, CPU resource type은 4개의 instance를 가짐

### lock, deadlock

- mutex lock, semaphore 같은 동기화 도구(lock)들이 deadlock의 흔한 원인
- lock 은 queue를 보호하거나 access를 보호하기 위해 사용됨
    - lock의 instance는 일반적으로 자체 resource class에 할당

### thread 의 resource 활용

- **Request** : thread가 resource를 요청
    - 바로 획득이 어려우면, 반드시 기다릴 것
    - `request()` system call, `open()` file, `allocate()` memory, `wait()` semaphore, `acquire()` mutex lock
- **Use** : thread가 resource를 사용
    - e.g. resource가 mutex lock이면, critical section 진입
- **Release** : thread가 resource를 반환
    - e.g. mutex lock을 release
    - `release()` system call, `close()` file, `free()` memory, `signal()` semaphore, `release()` mutex lock

### system table

- OS는 resource의 thread 할당 여부를 system table에 기록
- thread가 resource를 요청하면, resource type과 instance를 기록

### deadlock 발생

- 같은 set안에서 모든 thread가 특정 event를 _waiting_ (event는 같은 set안의 다른 thread에 의해서만 발생 가능)
- resource : logical, mutex lock, semaphore, ...
- event : network interface, IPC, ...
- dining-philosophers problem
    - resource : 젓가락
    - event : 다른 사람이 젓가락을 내려놓을 때
- **race condition을 예방하기위해 lock을 사용하는데, 이 lock이 deadlock을 발생시킬 수 있음을 유의**
