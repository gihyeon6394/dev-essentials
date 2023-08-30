# 5. IPC in Shared Memory Systems

- 프로세스 간의 통신을 공유 메모리 영역을 활용하여 구현
- 프로세스들은 공유 메모리 영역에 read/write
- 프로세스들은 공유 메모리의 같은 영역에 동시에 wirte 불가

### producer-consumer problem <sup>생산자-소비자 문제</sup>

- producer : 공유 메모리에 데이터를 쓰는 프로세스
    - e.g. server
- consumer : 공유 메모리에서 데이터를 읽는 프로세스
    - e.g. client

#### shared memroy를 통한 해결

- shared memmory 에 **buffer**를 배치
- producer는 buffer에 데이터를 쓰고, consumer는 buffer에서 데이터를 읽음
- producer와 consumer를 동기화
    - produce되지 않은 데이터를 consume하지 못하게 함
- unbounded buffer : 무한한 사이즈
    - consumer는 대기할 수 있지만, producer는 언제나 대기없이 쓰기 가능
- bounded buffer : 유한한 사이즈
    - consumer는 buffer가 비었으면 대기, producer는 buffer가 꽉차면 대기

#### C 예시

```C
#define BUFFER_SIZE 10

typedef struct {
    // .. buffer data
} item;

item buffer[BUFFER_SIZE];
int in = 0;
int out = 0;
```

- `in` : buffer의 다음 빈 공간
- `out` : buffer의 다음 읽을 데이터

```C
// producer

item next_produced;

while (true) {
    while (((in + 1) % BUFFER_SIZE) == out) {
        // do nothing
    }
    buffer[in] = next_produced;
    in = (in + 1) % BUFFER_SIZE;
}
```


