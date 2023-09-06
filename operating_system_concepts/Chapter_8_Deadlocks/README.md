# chapter 8 Deadlocks

- _every process in a set of processes is waiting for an event that can be caused only by another process in the set._
- multiporgramming 환경에서 1개 이상의 thread가 유한한 resource를 가지고 경쟁함
- waiting thread가 필요한 자원을 가지고있는 thread가 waiting이 되면 deadlock 발생
- OS는 deadlock 발생을 막지 않기 떄문에 programmer가 deadlock을 피하기 위해 프로그램을 작성해야함

> ### CHAPTER OBJECTIVES
>
> - mutex lock을 사용할 때, deadlock이 일어나는 상황
> - deadlock의 4가지 조건
> - resource 할당 그래프로 deadlock 상황 식별
> - deadlock 회피 : banker's algorithm
> - deadlock detection algorithm
> - deadlock recovery


--- 

1. [System Model](1_System_Model/README.md)
2. Deadlock in Multithreaded Applications
3. Deadlock Characterization
4. methods for Handling Deadlocks
5. Deadlock Prevention
6. Deadlock Avoidance
7. Deadlock Detection
8. Recovery from Deadlock
9. Summary
