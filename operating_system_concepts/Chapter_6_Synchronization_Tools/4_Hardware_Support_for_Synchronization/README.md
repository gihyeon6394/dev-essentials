# 4. Hardware Support for Synchronization

1. Memory Barriers
2. Hardware instructions
3. Atomic Variables

---

## 1. Memory Barriers

### Memory Model

computer architecture가 application에 제공할 메모리를 결정 하는 방법

- Strongly ordered memory model
    - 한 processor에 대한 메모리 수정이 모든 porcessor에게 즉시 보임
- Weakly ordered memory model
    - 한 processor에 대한 메모리 수정이 모든 processor에게 즉시 보이지 않음

### Memory Barriers, Memory Fence

- 강제로 메모리에 대한 변화가 있으면 다른 processor에게 즉시 보이게 하는 명령어
- computer architecture가 제공하는 명령어
- low-level 명령
- kernel 개발자들이 상호 배제 구현시 사용

````
// thread 1
// x를 memroy에 올리기 전에 flag를 먼저 올려야함

while (!flag)
    memory_barrier();
print x;

// thread 2
// x값을 flag 값보다 먼저 할당해야함
x = 100;
memory_barrier();
flag = true;
````

## 2. Hardware Instructions

### test and set

````
boolean test_and_set(boolean *target) {
    boolean rv = *target;
    *target = true;
    return rv;
}

/* ... */

boolean lock = false;

do {
    while (test_and_set(&lock))
        ; // do nothing

    /* critical section */
    
    lock = false;
    
    /* remainder section */
    
} while (true);
````

- `test_and_set()`
    - atomic하게 연산
    - 동시에 2개가 실행된다면, 임의의 순서로 연속적으로 실행됨

### compare and swap

````
int compare_and_swap(int *value, int expected, int new_value) {
    int temp = *value;
    
    if (*value == expected)
        *value = new_value;
        
    return temp;
}

/* ... */

int lock = 0;

while (true) {
  while (compare_and_swap(&lock, 0, 1) != 0)
    ; /* do nothing */
  
  /* critical section */
  
  lock = 0;
  
  /* remainder section */
}
````

- `compare_and_swap()`
    - atomic하게 연산
    - 동시에 2개가 실행된다면, 임의의 순서로 연속적으로 실행됨
    - **bounded waiting을 구현 못함**
- `lock`
    - global variable

#### bounded waiting을 구현한 compare and swap

````
boolean waiting[n];
int lock;

while(true){
  waiting[i] = true;
  key = 1;
  
  while(waiting[i] && key == 1)
    key = compare_and_swap(&lock, 0, 1);
  
  waiting[i] = false;
  
  /* critical section */
  
  j = (i + 1) % n;
  
    while((j != i) && !waiting[j])
        j = (j + 1) % n;
    
    if(j == i)
        lock = 0;
    else
        waiting[j] = false;
        
    /* remainder section */
    
}

````

## 3. Atomic Variables

- integer와 boolean 타입으로 만든 변수
- atomic하게 연산할 때 flag 값이 됨
- race가 발생하면 flag를 바꿈

````
// v : 공유변수 sequence 값
// temp : 내가 실행할 당시의 값
void increment(atomic int *v)
{
  int temp; 
  
  do {
    temp = *v;
  }
  
  while (temp != compare_and_swap(v, temp, temp+1)); // v == temp일때만 1증가하고 빠져나옴
}

...

increment(&sequence);
````
