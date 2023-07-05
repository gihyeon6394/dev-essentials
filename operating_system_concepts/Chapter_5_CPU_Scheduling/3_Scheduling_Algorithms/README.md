# 3. Scheduling Algorithms

1. First-Come, First-Served Scheduling
2. Shortest-Job-First Scheduling
3. Round-Robin Scheduling
4. Priority Scheduling
5. Multilevel Queue Scheduling

---

- CPU Scheduling : ready queue에 있는 process 중 CPU core를 할당해줄 process를 선택하는 것
- CPU Scheduling Algorithm : CPU Scheduling에 사용되는 알고리즘

## 1. First-Come, First-Served Scheduling <sup>FCFS</sup>

- 비선점
- 요청 순서대로 process가 CPU를 할당받음
- FIFO queue로 의해 관리
- ready queue에 들어온 PCB는 FIFO queue에 링크됨
- 실행상태에 돌입하면 FIFO queue에서 dequeue
- 장점 : 단순함
- 단점 : process 별 CPU Burst time의 variation이 크면 평균 대기시간이 길어짐

#### 동작 예시

- P1 : 24ms
- P2 : 3ms
- P3 : 3ms

<img src="img_1.png"  width="50%"/>

- 평균 대기시간 :  17ms
- p2 -> p3 -> p1 순이면 평균 대기시간 3ms

<img src="img_2.png"  width="50%"/>

- convoy effect : CPU burst time이 긴 process가 ready queue에 들어오면, 그 process를 기다리는 process들이 모두 대기시간이 길어짐
    - burst time이 짧은 process가 먼저 들어와야 해결할 수 있는 문제
- interactive system에서는 사용하기 어려움
    - interactive system 는 Process가 CPU를 적절한 간격으로 나눠써 할당받음

## 2. Shortest-Job-First Scheduling <sup>SJF</sup>

- shortest-next-CPU-burst algorithm
- 선점, 비선점 가능
- 다음 실행할 process 중 가장 짧은 CPU Burst time을 가지는 process를 선택
    - 같을 시 FCFS를 사용
- 평균 대기시간을 줄이고 싶을 때 적합
- 단점 : 다음 CPU Burst time을 알 수 없을 때
    - 예측을 통해 해결

#### 동작 예시

- P1 : 6ms
- P2 : 8ms
- P3 : 7ms
- P4 : 3ms

<img src="img_3.png"  width="50%"/>

- 평균 대기시간 : 7ms
    - FCFS 일 때는 10.25 ms

#### CPU Burst time 예측 방법 : exponential average

- exponential average <sup>지수 평균</sup>을 활용하여 다음 CPU Burst time을 모를 때 예측
    - 다음 process의 CPU Burst time은 이전 process의 CPU Burst time과 비슷

#### SJF는 선점, 비선점이 가능


## 3. Round-Robin Scheduling

## 4. Priority Scheduling

## 5. Multilevel Queue Scheduling

## 6. Multilevel Feedback Queue Scheduling
