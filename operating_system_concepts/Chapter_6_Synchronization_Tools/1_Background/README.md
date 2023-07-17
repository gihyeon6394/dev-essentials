# 1. Background

- 동시성 : 프로세스는 shceduler에 의해 부분적으로 완료하고 다른 프로세스에게 CPU를 양도할 수 있음
- 병렬 : 프로세스는 서로 다른 Core에서 병렬적으로 실행될 수 있음

#### producer-consumer problem, 생산자 소비자 문제

예시 - 유한 버퍼를 공유 메모리로 사용

````
/* producer */
while(ture){

    while(count == BUFFER_SIZE); // do nothing

    buffer[in] = next_produced;
    in = (in + 1) % BUFFER_SIZE;
    count++;
}

/* consumer */
while(true){
    while(count == 0); // do nothing
    
    next_consumed = buffer[out];
     out = (out + 1) % BUFFER_SIZE;
     count--;
     
}
````

- `count` 변수를 사용해 현재 버퍼의 사이즈를 유지

#### 문제점

- 생산자와 소비자가 concurrent하게 실행된다고 할 때, `count` 변수를 동시에 읽고 쓰는 경우 문제가 발생할 수 있음

생산자와 소비자가 item을 하나씩 생산 / 소비할 경우

| 순번 | producer/consumer | 수행                        | 결과값           |
|----|-------------------|---------------------------|---------------|
| 1  | producer          | register1 = count         | register1 = 5 |  
| 2  | producer          | register1 = register1 + 1 | register1 = 6 |  
| 3  | consumer          | register2 = count         | register2 = 5 |   
| 4  | consumer          | register2 = count - 1     | register2 = 4 |  
| 5  | producer          | count = register1         | count = 6     |   
| 6  | consumer          | count = register2         | count = **4** |   

### race condition (경쟁 상태)와 process synchronization (프로세스 동기화)

- n개의 프로세스가 같은 데이터에 접근하여 concurrently하게 값을 수정할 때, 실행 결과가 실행 순서에 바뀔 수 있는 것
- 프로세스 동기화 필요성, synchronization
    - 경쟁상태 방지를 위해, 동시에 하나의 프로세스만이 공유 데이터에 접근할 수 있도록 해야함
    - 프로세스 동기화를 통해 구현


