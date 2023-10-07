# chapter 9 Main Memory

- memeory 공유 : memory에 올라간 process가 많을수록 성능 향상
- memory 관리 알고리즘, 기계적 접근부터 paging 전략까지
- 알고리즘은 하드웨어 설계 등에 의존

> ### CHAPTER OBJECTIVES
>
> - 논리, 물리 주소의 차이점, memory management unit의 역할
> - memory 연속 할당의 최적, 최악 전략
> - 메모리 내부, 외부 단편화의 구분
> - paging system에서 논리 메모리 주소를 물리주소로 변환, translation look-aside buffer (TLB)
> - hierarchical paging, hashed paging, and inverted page tables
> - IA-32, x86-64, and ARMv8 아키텍쳐의 주소변환


---

1. [Background](1_Background/README.md)
2. [Contiguous Memory Allocation](2_Contiguous_Memory_Allocation/README.md)
3. Paging
4. Structure of the Page Table
5. Swapping
6. Example : Intel 32 and 64-bit Architectures
7. Example : ARMv8 Architecture
8. Summary
