# 9. Other Considerations

1. Prepaging
2. Page Size
3. TLB Reach
4. Inverted Page Tables
5. Program Structure
6. I/O Interlock and Page Locking

---

- paging system의 가장 메인 이슈는 replacement algorithm, allocation policy
- 그 외의 고려사항들은?
    - prepaging
    - page size
    - TLB reach
    - inverted page tables
    - program structure
    - I/O interlock and page locking

## 1. Prepaging

- **Prepaging** : 메모리에서 필요할 일정량 (또는 전체)의 page를 미리 한번에 가져오는 전략
- e.g. working-set model에서 미리 process마다 page 리스트를 만들어두고, 이 리스트에 있는 page를 미리 가져옴
    - process가 중단되면 working set을 기억해두었다가 다시 시작할때 전체 page 리스트를 가져옴
- 고려사항 : prepaging 비용 vs page-fault 비용
- Linux `readahead()` system call : contents의 파일을 메모리에 prefetch

## 2. Page Size

## 3. TLB Reach

## 4. Inverted Page Tables

## 5. Program Structure

## 6. I/O Interlock and Page Locking