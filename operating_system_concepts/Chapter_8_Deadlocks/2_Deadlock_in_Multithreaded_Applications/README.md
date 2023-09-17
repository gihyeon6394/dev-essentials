# 2. Deadlock in Multithreaded Applications

1. Livelock

---

- POSIX mutex lock을 사용할 때 deadlock 상황
- `pthread_mutex_init()` : mutex lock 초기화
- `pthread_mutex_lock()` : mutex lock 획득
    - 획득할 때까지 blocking
- `pthread_mutex_unlock()` : mutex lock release

````
/* lock 2개 initialize */
pthread_mutext_t first_mutex;
pthread_mutext_t second_mutex;

pthread_mutex_init(&first_mutex, NULL);
pthread_mutex_init(&second_mutex, NULL);

/* thread_one runs this function */
void *do_work_one(void *param) {
   
    /* Acquire first mutex lock 1 -> 2*/
    pthread_mutex_lock(&first_mutex);
    pthread_mutex_lock(&second_mutex);
    
    /* critical section for thread one */
    
    pthread_mutex_unlock(&second_mutex);
    pthread_mutex_unlock(&first_mutex);
    
    pthread_exit(0);
}

/* thread_two runs this function */
void *do_work_two(void *param) {
   
    /* Acquire second mutex lock 2 -> 1*/
    pthread_mutex_lock(&second_mutex);
    pthread_mutex_lock(&first_mutex);
    
    /* critical section for thread two */
    
    pthread_mutex_unlock(&first_mutex);
    pthread_mutex_unlock(&second_mutex);
    
    pthread_exit(0);
}
````

## Livelock

- deadlock과 비슷한 liveness failure의 또 다른 형태
- 2개 이상의 thread가 서로 다른 이유로 진행되지 못하는 상태
- e.g. 복도에서 서로 마주치는 2개의 사람이 서로 양보하려고 하는데, 계속해서 양보하다가 결국 아무도 지나가지 못하는 상황
- `pthread_mutex_trylock()` : mutex lock 획득
    - non-blocking, 획득할 수 없으면, `EBUSY` error return
- 예방 : 각 thread는 random 회수만큼만 재시도
    - e.g. Ethernet network에서 collision이 발생하면, random 회수만큼만 재시도하고 **_backoff_**

````
/* thread_one runs this function */
void *do_work_one(void *param) {
   int done = 0; // 완료 여부
   
   while (!done) {
   `  pthread_mutex_lock(&first_mutex);
     
     if(pthread_mutex_trylock(&second_mutex)){
        /* critical section for thread one */
        pthread_mutex_unlock(&second_mutex);
        pthread_mutex_unlock(&first_mutex);
        done = 1;
     }
     else {
        pthread_mutex_unlock(&first_mutex);
     }
    
    pthread_exit(0);
   }
}

/* thread_two runs this function */
void *do_work_two(void *param) {
   int done = 0; // 완료 여부
   
   while (!done) {
   `  pthread_mutex_lock(&second_mutex);
     
     if(pthread_mutex_trylock(&first_mutex)){
        /* critical section for thread two */
        pthread_mutex_unlock(&first_mutex);
        pthread_mutex_unlock(&second_mutex);
        done = 1;
     }
     else {
        pthread_mutex_unlock(&second_mutex);
     }
    
    pthread_exit(0);
   }
}
````
