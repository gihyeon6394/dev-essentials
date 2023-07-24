# chapter 6 Synchronization Tools

cooperating process (협력 프로세스) 란 실행 중인 프로세스가 다른 프로세스에게 영향을 주거나 받을 수 있는 프로세스를 말함
데이터 일관성을 해치지 않고, logical adress space를 사용하여 협력 프로세스를 운용하는 방법

> ### CHAPTER OBJECTIVES
>
> - critical-section (임계영역) 과 race condition (경쟁상태) 예시
> - 임계영역의 hardware solution : memory barriers, compare-and-swap, atomic instructions
> - mutex locks, semaphores, monitors, condition variables를 사용하여 임계영역을 해결하는 방법
> - 낮은, 보통, 높은 수준의 경합 상황에서의 해결 도구

---

1. [Background](1_Background/README.md)
2. [The Critical-Section Problem](2_The_Critical_Section_Problem/README.md)
3. [Peterson's Solution](3_Peterson's_Solution/README.md)
4. Hardware Support for Synchronization
5. Mutex Locks
6. Semaphores
7. Monitors
8. Liveness
9. Evaluation
10. Summary
