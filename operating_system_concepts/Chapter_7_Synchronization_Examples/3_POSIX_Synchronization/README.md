# 3. POSIX Synchronization

1. POSIX Mutex Locks
2. POSIX Semaphores
3. POSIX Condition Variables

---

- POSIX API는 kernel이 아닌 programmer level의 동기화

## 1. POSIX Mutex Locks

- Pthread API 동기화의 기본
- `pthread_mutex_t` type의 mutex를 사용
- `pthread_mutex_init()` : mutex 초기화

```c
#include <pthread.h>

pthread_mutex_t mutex;

/* create and initialize mutex lock */
pthread_mutex_init(&mutex, NULL);
```

- `pthread_mutex_lock()` : mutex lock
    - 다른 thread가 'pthread_mutex_unlock()` 호출할 때까지 block
- `pthread_mutex_unlock()` : mutex unlock

```c
/* acquire the mutex lock */
pthread_mutex_lock(&mutex);

/* critical section */

/* release the mutex lock */
pthread_mutex_unlock(&mutex);
````

## 2. POSIX Semaphores

- **POSIX SEM** 확장
- POSIX 표준은 아니지만 많은 system에서 Semaphore를 지원
- **named**, **unamed** semaphore 지원
    - 생성방법, process 간의 공유하는 방식에 차이
    - kernel version 2.6 ~ Linux system에서 named, unamed 지원

### 2.1 POSIX Named Semaphores

- 장점 : 다른 process 들이 semaphore name 을 통해 쉽게 접근 가능
- Linux, macOS 에서 지원

```c
#include <semaphore.h>
sem_t *sem;

/* Create the semaphore and initialize its value to 1 */
sem = sem_open("SEM", O_CREAT, 0666, 1);
```    

- `sem_open()` : named semaphore 생성
- `"SEM"` : semaphore의 이름
- `O_CREAT` : semaphore가 존재하지 않으면 생성
- `0666` : semaphore의 permission
- `1` : semaphore의 초기값

```c
/* acquire the semaphore */
sem_wait(sem);

  /* critical section */

/* release the semaphore */
sem_post(sem);
```

### 2.2 POSIX Unnamed Semaphores

- `sem_init()` :  unnamed semaphore 생성
    - paramter 1 : semaphore pointer
    - parameter 2 : semaphore의 공유 level
    - parameter 3 : semaphore의 초기값

```c
#include <semaphore.h>
sem_t sem;

/* Create the semaphore and initialize its value to 1 */
sem_init(&sem, 0, 1);
```

- paramter 2 `0` : semaphore를 생성한 thread 내에서만 공유 가능
    - 0보다 크면 같은 공유 메모리 안에서 semaphore 공유 가능

```c
/* acquire the semaphore */
sem_wait(&sem);

  /* critical section */
  
/* release the semaphore */
sem_post(&sem);
```

## 3. POSIX Condition Variables

- `pthread_cond_t` type의 condition variable 사용
- `pthread_cond_init()` : condition variable 초기화

```c
pthread_mutex_t mutex;
pthread_cond_t cond_var;

pthread_mutex_init(&mutex, NULL);
pthread_cond_init(&cond_var, NULL);
```

- `pthread_cond_wait()` : condition variable을 waiting

```c
pthread_mutex_lock(&mutex);
while(a != b)
    pthread_cond_wait(&cond_var, &mutex);
    
pthread_mutex_unlock(&mutex);
```

- `pthread_cond_signal()` : waiting thread를 깨움

```c
pthread_mutex_lock(&mutex);
a = b;
pthread_cond_signal(&cond_var);
pthread_mutex_unlock(&mutex);
```

