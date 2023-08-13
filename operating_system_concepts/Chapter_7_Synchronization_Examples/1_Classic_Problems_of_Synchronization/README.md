# 1. Classic Problems of Synchronization

1. The Bounded-Buffer Problem
2. THer Readers-Writers Problem
3. The DIning-Philosophers Problem

---

## 1. The Bounded-Buffer Problem

````
int n;
semaphore mutex = 1;
semaphore empty = n;
semaphore full = 0;
````

- buffer : n개의 고정된 크기를 가진 공유 메모리
- `mutex` : 공유 메모리에 대한 접근, 상호 배제를 제공하는 binary semaphore
- `empty` : buffer에 쓸 수 있는 공간의 수를 나타내는 semaphore
- `full` : buffer에 읽을 수 있는 공간의 수를 나타내는 semaphore

````
// producer
while(true){
    ...
    /* produce an item in next_produced */
    ...
    wait(empty);
    wait(mutex);
    ...
    /* add next_produced to the buffer */
    ...
    signal(mutex);
    signal(full);
}

// consumer
while (true) {
    wait(full);
    wait(mutex);
    ... 
    /* remove an item from buffer to next_consumed */
    ...
    signal(mutex);
    signal(empty);
    ...
    /* consume the item in next_consumed */
    ...
}

````

## 2. The Readers-Writers Problem

- data에 대한 행위를 *reader*와 *writer*로 구분
- writer와 다른 process (reader or writer)가 동시에 접근하면 문제 발생
- readers - writers problem : writer가 공유 데이터에 접근할 때는 배타적으로 접근
- first readers-writers problem : writer가 허가를 받으면, 다른 reader를 선점
- second readers-writers problem : writer가 ready 상태가 되면 가능한 빨리 접근하도록 함
    - writer가 waiting일 때는 reader가 접근 불가능

### starvation, 기아 상태

- first problem에서는 writer가 기아 상태에 빠질 수 있음
- second problem에서는 reader가 기아 상태에 빠질 수 있음

## 3. The Dining-Philosophers Problem

