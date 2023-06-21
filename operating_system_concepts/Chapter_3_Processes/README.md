# chapter 3 Processes

> ### CHAPTER OBJECTIVES
>
> - 프로세스의 분리된 컴포넌트들을 식별하고, OS 내에서 표현되고 스케쥴링되는 방법
> - 프로세스의 생성과 종료 방법, 올바른 System call을 사용하여 프로그램을 개발하는 방법
> - 프로세스간의 커뮤니케이션 비교, shared memory vs message-passing
> - pipe와 POSIX 공유 메모리를 이용여 프로세스간의 통신하는 프로그램 설계
> - client-server 커뮤니케이션 : 소켓, remote procedure call
> - Linux OS와 상호작용하는 커널 모듈설계

현대 컴퓨터는 다수의 프로그램을 load 하고 동시에 실행함  
컴퓨터가 복잡해짐에 따라 프로세스를 관리할 필요성은 더 커짐

---

1. [Process Concept](1_Process_Concept/README.md)
2. [Process Scheduling](2_Process_Scheduling/README.md)
3. [Operations on Processes](3_Operations_on_Processes/README.md)
4. [Interprocess Communication](4_Interprocess_Communication/README.md)
5. [IPC in Shared Memory Systems](5_IPC_in_Shared_Memory_Systems/README.md)
6. IPC in Message-Passing Systems
7. Examples of IPC Systems
8. Communication in Client-Server Systems
9. Summary
