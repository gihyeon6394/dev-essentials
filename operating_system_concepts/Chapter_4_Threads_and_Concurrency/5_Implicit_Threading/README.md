# 5. Implicit Threading

1. Thread Pools
2. Fork Join
3. OpenMP
4. Grand Central Dispatch
5. Intel Threading Building Blocks

---

Multicore Processing의 발전으로, 수백~수천개의 thread를 포함하는 application이 늘어나고 있음

#### implicit threading

- compiler, runtime library에 의해 thread를 생성하고 관리하는 것.
    - **개발자가 아니라**
- implicit threading의 전략을 잘 사용하려면,
    - 개발자는 병렬으로 실행할 task를 잘 정의해야함
- 개발자가 Task를 잘 정의만하면, library가 thread를 생성하고 관리함

## 1. Thread Pools

#### thread pool이 해결한 것

- 시간 소모 : thread를 생성하는데 시간
- 리소스 <sub>CPU, Memory</sub> 소모 :  동시성의 요청을 많이 보내는만큼 thread가 생김. thread 수에 제한이 없음

#### thread pool 메커니즘

- 시스템 시작 시, 특정 수의 thread를 생성해 pool을 만들어둠
- pool에 배치된 thread들은 task를 기다림
- server가 요청하면, thread를 생성하지 않고, thread pool에 thread를 요청함
    - 가용할 수 있는 thread가 없다면 기다림
- thread가 task를 종료하면 pool에 돌아가 task를 기다림
- task가 비동기 작업일 때 효과적

#### thread pool의 장점

- thread 생성 시간이 없음
- 리소스 절약 : 존재하는 thread의 최대 개수에 제한을 둠
    - 시스템이 지원 불가능한 개수의 thread 생성을 방지
- task를 작성하는 것을 task를 실행하는 것으로부터 분리
    - 각각 다른 전략을 취할 수 있음 <sub>task 실행시간에 지연, 주기 등을 준다던가 등<sub>

## 2. Fork Join

## 3. OpenMP

## 4. Grand Central Dispatch

## 5. Intel Threading Building Blocks

