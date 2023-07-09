# 6. Real-Time CPU Scheduling

1. Minimizing Latency
2. Priority-Based Scheduling
3. Rate-Monotonic Scheduling
4. Earliest-Deadline-First Scheduling
5. Proportional-Share Scheduling
6. POSIX Real-Time Scheduling

---

#### Soft real-time system vs Hard real-time system

- Soft real-time system
    - real-time process의 schedule 시점을 보장하지 않음
    - 다른 process보다 우선순위를 가짐
- Hard real-time system
    - deadline을 보장해야 함
    - deadline을 넘어서면 service 제공하지 않은 것과 동일

## 1. Minimizing Latency

#### Event latency

<img src="img.png"  width="40%"/>

- event latency : event가 발생한 시점부터 service가 가능한 시점까지의 시간
- real-time system은 event-driven nature
- event가 발생하면 system은 응답하고, service가 가능해야함
- event 마다 다른 latency를 요구함
- latency에 영향을 주는 요소
    - Interrupt latency
    - Dispatch latency

#### Interrupt latency

<img src="img_1.png"  width="40%"/>

- interrupt가 CPU에 도착부터 interrupt를 처리하기 시작할 때까지의 시간
- interrupt latency = OS가 현재 실행중인 명령을 완료 + interrupt type을 결정 + 최근 process의 상태를 저장
- interrupt latency 이후 ISR <sup>interrupt service routine</sup> 을 사용하여 interrupt를 처리
- real-time system에서 interrupt latency를 최소화하는 것이 중요
    - hard real-time system에서는 엄격하게 제한

#### Dispatch latency

<img src="img_2.png"  width="40%"/>

- Dispatcher가 현재 process를 stop하고, 다른 process를 실행하기까지 걸린 시간
- hard real-time system에선 몇 ms 정도
- confilct phase
    - kernel에서 실행 중인 모든 process에 대한 선점
    - 낮은 우선순위 process가 높은 우선순위 process에게 resource를 release

## 2. Priority-Based Scheduling

## 3. Rate-Monotonic Scheduling

## 4. Earliest-Deadline-First Scheduling

## 5. Proportional-Share Scheduling

## 6. POSIX Real-Time Scheduling

