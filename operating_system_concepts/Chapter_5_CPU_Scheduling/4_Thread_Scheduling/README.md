# 4. Thread Scheduling

1. Contention Scope
2. Pthread Scheduling

---

대부분의 현대 OS에서는 kernel-level thread 는 OS가 scheduling 하고, user-level thread 는 library가 scheduling   
CPU에서 실행하기 위해서는 LWP와를 사용해서 간접적으로 kernel-level thread와 mapping

## 1. Contention Scope

#### process contention scope <sup>PCS</sup>

- thread library가 user-level thread를 LWP에 scheduling
    - 실제 CPU에서 실행하기 위해서는 OS가 LWP 를 CPU Core 에 scheduling하는 것이 추가로 필요함
- scheduling 대상 : 같은 process 안의 thread
- Many-to-One model, Many-to-Many model
- 우선순위에 의해 scheduling
    - user-level thread의 우선순위는 프로그래머에 의해 결정
- 실행 중인 thread 선점 가능

#### system contention scope <sup>SCS</sup>

- kernel이 kernel-level thread를 CPU에 scheduling
- scheduling 대상 : system 안의 모든 thread
- Windows, Linux는 SCS만 사용함

## 2. Pthread Scheduling

- `PTHREAD_SCOPE_PROCESS` : PCS scheduling
    - Many-to-Many model에서 user-level thread를 LWP에 scheduling
    - LWP들은 thread library에 의해 유지
- `PTHREAD_SCOPE_SYSTEM` : SCS scheduling
    - Many-to-Many model : LWP를 생성하고, 각 user-level thread를 LWP에 One-to-One mapping
- `pthread_attr_setscope()` : thread의 contention scope를 설정
- `pthread_attr_getscope()` : thread의 contention scope를 얻음

```c
#include <pthread.h>
#include <stdio.h>

#define NUM THREADS 5

int main(int argc, char *argv[])
{
  int i, scope;
  pthread t tid[NUM THREADS];
  pthread attr t attr;
  
  /* get the default attributes */
  pthread attr init(&attr);
  
  /* first inquire on the current scope */
  if (pthread attr getscope(&attr, &scope) != 0)
    fprintf(stderr, "Unable to get scheduling scope∖n");
  else {
    if (scope == PTHREAD SCOPE PROCESS)
      printf("PTHREAD SCOPE PROCESS");
    else if (scope == PTHREAD SCOPE SYSTEM)
      printf("PTHREAD SCOPE SYSTEM");
    else
      fprintf(stderr, "Illegal scope value.∖n");
  }
  
  /* set the scheduling algorithm to PCS or SCS */
  pthread attr setscope(&attr, PTHREAD SCOPE SYSTEM);
  
  /* create the threads */
  for (i = 0; i < NUM THREADS; i++)
    pthread create(&tid[i],&attr,runner,NULL);
  
  /* now join on each thread */
  for (i = 0; i < NUM THREADS; i++)
    pthread join(tid[i], NULL);
  }
  
  /* Each thread will begin control in this function */
  void *runner(void *param)
  {
    /* do some work ... */
  
  pthread exit(0);
}
```
