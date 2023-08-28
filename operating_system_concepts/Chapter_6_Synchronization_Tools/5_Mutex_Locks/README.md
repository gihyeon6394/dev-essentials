# 5. Mutex Locks, spin lock

### Mutex Lock?

````
while(true){
  acquire lock;
    /* critical section */
    
  release lock;
    /* remainder section */
}
````

- high-level에서 임계영역 문제를 해결하기 위한 도구
    - high-level : programmer level
    - low-level : kernel level
- mutex lock은 0 또는 1의 값을 가짐
    - 0 : lock이 해제된 상태
    - 1 : lock이 획득된 상태
    - 임계영역을 종료할 때 lock을 해제 (release)

### Mutex Lock의 구현

- `acquire()` : lock을 획득하는 함수
- `release()` : lock을 해제하는 함수
- 변수 `available` : lock 획득 가능 여부
- CAS로 구현 가능

````

while (true){

  acquire();
    /* critical section */
  
  release();
      /* remainder section */    

}

acquire(){
    while (available == 0)
        ; // busy wait
    available = 0;
}

release(){
    available = true;
}
````

> #### LOCK CONTENTION
>
> - Lock contention, 경합상태 : 두 개 이상의 프로세스가 lock을 획득하기 위해 경쟁하는 상황
> - 높은 경합상태는 많은 thread가 lock을 획득하기 위해 기다리는 상황
> - 높은 경합상태가 많을 수록 동시성 app의 전반적인 성능이 저하됨

### Mutex Lock 특징

- busy waiting : lock을 획득하기 위해 기다림
    - single-core system에서 multiprogramming 시 문제됨
    - CPU를 점유하고있어, 다른 프로세스가 실행될 수 없음
- spin lock
    - process 가 lock을 획득하기 위해 _spin_ 중
    - lock을 기다리는 행위에서는 context switch가 일어나지 않는 장점
    - short duration : lock을 짧은 기간동안 보유함
    - 현대 multi-core system에서 많이 사용 중

> #### SHORT DURATION SPIN LOCKS
>
> - multiprocessor system에서 lock이 짧은 기간동안 보유되어야할 떄
> - 실행 순서 : context switch -> lock 획득 및 임계영역 실행 -> context switch
> - 앞 뒤 2개의 context switch의 합이 임계영역을 실행하는 시간보다 짧을 경우 spin lock 사용
