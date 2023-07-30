# 6. Semaphores

1. Semaphore Usage
2. Semaphore Implementation

---

#### Semaphores

- Integer value `S`로 표현
- `wait()`, `signal()` 두 개의 atomic operation으로만 접근 가능한 값
    - `wait()` :  `P`로 표현, "to test"
    - `signal()` : `V`로 표현, "to increment"

````
wait(S){
     while(S <= 0) 
     ; // busy wait
     
     S--;
}

...

signal(S){
     S++;
}
````

## 1. Semaphore Usage

- counting semaphore : `S` 값 범위에 제한 없음
- binary semaphore : `S` 값이 0 또는 1, Mutex Lock과 유사
    - mutex lock을 제공하지 않는 시스템에서는 binary semaphore를 사용하여 상호 배제를 제공

### Counting Semaphore

- `wait()` : process가 리소를 사용하기 원할 때, semaphore 값을 감소시킴
- `signal()` : process가 리소를 사용을 마치면, semaphore 값을 증가시킴
- `S` 가 0이면, 모든 리소스가 사용 중이므로 `wait()`를 호출한 process는 block됨
    - `S`가 0 보다 커지면 block된 process는 깨어남

### 예시

- Process P1, P2가 있음
- P1은 명령 S1, P2는 명령 S2를 수행
- **S2는 S1이 완료된 다음 실행되어야함**
- `synch` : P1과 P2가 공유하는 Semaphore

````
S1;
signal(synch);

...
wait(synch);
S2;
````

## 2. Semaphore Implementation

### suspend operation

- process가 `wait()`에서 busy wait에 들어가기 직전에 스스로 실행을 연기
- process를 waiting queue에 두고, process 상태를 waiting으로 변경
    - **CPU scheduler는 다른 process를 실행할 수 있음**
- `wakeup()` :  `siganl()`이 실행되면 waiting process는 다시 실행
    - process를 waiting 상태에서 ready 상태로 변경
    - ready queue에 enqueue

### 구현

```c
typedef struct {
     int value;
     struct process *list;
} semaphore;
```

- `list` : semaphore를 기다리는 process들의 리스트

````
wait(semaphore *S){
     S->value--;
     
     if(S->value < 0){
          add this process to S->list;
          sleep();
     }
}

...

signal(semaphore *S){
     S->value++;
     
     if(S->value <= 0){
          remove a process P from S->list;
          wakeup(P);
     }
}
````

- **Semaphore 값은 음수가 될 수 있음**
- `list`는 PCB를 가지는 link field를 가짐
    - link field에 PCB pointer를 저장

### 임계영역

- Semaphore 명령어는 반드시 atomic operation이어야 함
    - `wait()`와 `signal()`
- Single-processor에서는 간단히 해결
    - 현재 실행중인 process는 interrupt가 enable될때까지 실행
- multi-core 환경
    - 모든 core에 interupt를 diabled 해도, interleave가 발생할 수 있음
    - 모든 core에 interrupt를 disabled하는 작업은 어렵고, 성능이 저하됨
    - 따라서, SMP 시스템은 `CAS`와 spinlock를 추가해서 사용

  
