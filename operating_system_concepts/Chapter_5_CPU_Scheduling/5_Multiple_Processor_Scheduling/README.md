# 5.Multi-Processor Scheduling

1. Approaches to Multiple-Processor Scheduling
2. Multicore Processors
3. Load Balancing
4. Processor Affinity
5. Heterogeneous Multiprocessing

---

Multiple CPU 환경에서는 load sharing이 가능해짐

#### multiprocessor architecture

- multiple physical processors, each contained one single-core CPU
- Multicore CPUs
- Multithreaded cores
- NUMA systems
- Heterogeneous multiprocessing

## 1. Approaches to Multiple-Processor Scheduling

#### asymmetric multiprocessing

- 하나의 processor <sup>master server</sup> 모든 scheduling을 담당
- 다른 processor <sup>slave server</sup>는 user code 만 실행
- master server가 bottleneck 되어 전체적인 시스템 성능 저하 가능성

#### symmetric multiprocessing <sup>SMP</sup>

- 각 processor들이 스스로 scheduling
- processor의 scheduler는 ready queue 보고, 실행할 thread를 선택
- 거의 모든 modern OS에서 사용 <sub>Windows, Linux, Android, mac OS</sub>

#### thread 관리 전략

<img src="img.png"  width="50%"/>

1. common ready queue : 모든 thread를 공용 ready queue에 관리
    - race condition 발생 가능성
        - locking mechanism 필요
    - bottleneck
2. private queue : 각 processor마다 private ready queue를 관리
    - shared queue와 같은 성능 문제 없음
    - SMP 구현에 일반적으로 사용됨
    - cache memeory 사용에 효과적
    - balancing algorithm : processor 들 간의 업무량 조절 필요

## 2. Multicore Processors

## 3. Load Balancing

## 4. Processor Affinity

## 5. Heterogeneous Multiprocessing
