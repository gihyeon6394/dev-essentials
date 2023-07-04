# 2. Scheduling Criteria

### CPU 알고리즘에 고려되는 기준

- CPU utilization
    - CPU는 항상 바빠야함
    - 40~90% 정도가 적당
    - Linux `top` 명령어로 확인 가능
- Throughput
    - 단위 시간당 완료된 프로세스 수
- Turnaround time
    - ready queue에서 wait상태 + CPU에서 running 상태 + I/O wait상태
    - 해당 process를 얼마나 오랫동안 실행했는가
- Waiting time : ready queue에서 wait상태로 머무른 시간
- Response times
    - 최초 응답까지 걸린 시간

### 알고리즘의 평가

- CPU utilization과 throughput은 높을수록 좋고, 나머지는 낮을수록 좋음
    - 평균 response time을 줄이는 것이 목표
- 어떤 경우에는 특정 기준이 더 우선시 될 수 있음
    - PC : response time의 유동성을 최소화 하는 것이 우선시