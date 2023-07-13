# 8. Algorithm Evaluation

1. Deteministic Modeling
2. Queueing Models
3. Simulations
4. Implementation

---

#### scheduling 알고리즘 평가 기준

- CPU utilization 최대화
    - 최장 응답 시간 300ms
- throughput 최대화
    - 전체 실행 시간 대비 turnarround 시간이 선형 비례

## 1. Deterministic Modeling

- analytic evaluation : 주어진 알고리즘과 작업량으로 성능을 나타내는 공식이나 숫자를 만듦
- Deterministic Modeling은 analytic evaluation의 한 종류
- 쉽고 빠르게 계산할 수 있음
- 숫자로 알고리즘 성능을 비교할 수 있음

#### modeling 방법

| Process | Burst Time |
|:-------:|:----------:|
|   P1    |     10     |
|   P2    |     29     |
|   P3    |     3      |
|   P4    |     7      |
|   P5    |     12     |

##### FCFS

<img src="img.png"  width="40%"/>

- average waiting time = 28ms

##### SJF

<img src="img_1.png"  width="40%"/>

- average waiting time = 13ms

#### RR (quantum = 10ms)

<img src="img_2.png"  width="40%"/>

- average waiting time = 23ms

## 2. Queueing Models

- CPU Burst와 I/O Burst의 분포를 측정함
- 한계
    - 복잡한 알고리즘에 적용하기 힘듦
    - service rate, arrival rate 등이 숫자로서 다뤄질수 있어야함

#### queueing-network analysis

- CPU : ready queue를 가진 서버
- I/O : dvice queue를 가진 서버
- arrival rate, service rate를 사용하여 이용률, 평균 queue 길이, 평균 대기시간 등을 계산함

#### Little’s formula

````
n = λ × W.
````

- n : 평균 장시간 대기열 길이
- λ : 평균 도착 률 (3 : 초당 3개 프로세스 도착)
- W : 평균 queue에서 대기 시간

## 3. Simulations

## 4. Implementation
