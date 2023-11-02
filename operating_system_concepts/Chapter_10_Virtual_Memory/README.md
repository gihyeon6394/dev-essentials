# 10. Virtual Memory

- Virtual Memory : 프로세스 전체를 메모리에 올리지 않고 실행하는 기술
- 이점 1 : 프로그램이 메모리 크기보다 커도 가능
- 이점 2 : 효율적인 프로세스 생성 메커니즘 제공

> ### CHAPTER OBJECTIVES
>
> - virtual memory의 개념, 이점
> - page가 메모리에 loading되는 과정
> - FIFO, optimal, LRU page 교체 알고리즘 적용
> - 프로세스 집합과 프로그램 locality의 관계
> - Linux, Windows 10, Solaris 의 virtual memory 구현
> - C program에서 virtual memory 관리


---

1. [Background](1_Background/README.md)
2. [Demand Paging](2_Demand_Paging/README.md)
3. Copy-on-Write
4. Page Replacement
5. Allocation of Frames
6. Thrashing
7. Memory C
8. Allocating Kernel Memory
9. Other Considerations
10. Operating-System Examples
11. Summary