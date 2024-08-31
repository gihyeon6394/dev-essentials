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
3. [Copy-on-Write](3_Copy_on_Write/README.md)
4. [Page Replacement](4_Page_Replacement/README.md)
5. [Allocation of Frames](5_Allocation_of_Frames/README.md)
6. [Thrashing](6_Thrashing/README.md)
7. [Memory Compression](7_Memory_Compression/README.md)
8. [Allocating Kernel Memory](8_Allocating_Kernel_Memory/README.md)
9. [Other Considerations](9_Other_Considerations/README.md)
10. [Operating-System Examples](10_Operating_System_Examples/README.md)
11. [Summary](11_Summary/README.md)