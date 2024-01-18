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

- 새로운 머신을 설계할 때 최적의 페이지 사이즈를 결정해야함
- 2의 거듭제곱으로 정의됨 (4096 ~ 4194304 bytes)
- 페이지 사이즈가 작으면 -> 내부 fragmentation, locality 이득
- 페이지 사이즈가 크면 -> 테이블 사이즈, I/O time 이득
- 역사적으로 트렌드는 더 큰 page size

#### 고려사항 : page table 크기

- 페이지 사이즈가 작을수록 page 수가 증가
- 가상 메모리 4MB가 주어지면, 1024 bytes(1 KB) 페이지 사이즈는 4096개의 page 수를 가짐
    - 실제로는 8192 bytes(8 KB) 페이지 사이즈로 512개의 page 수를 가짐
    - 각 active process가 자신의 page table의 복사본을 가져야하기 떄문 (* 2)

#### 고려사항 : page read/write 시간, 페이지 크기

- HDD이면 I/O 시간은 탐색, 대기, 전송으로 구성 (seek, latency, transfer)
- 탐색 시간이 페이지 크기와 연관
- 총 I/O 시간에서 탐색 시간은 0.01 %
- 페이지 크기를 늘려도 I/O 시간은 크게 변화하지 않음

#### 고려사항 : 페이지 크기

- 페이지 크기가 줄어들면 전체 I/O가 줄어듦 (locality 증가)
- 각 페이지의 program locality 정확도가 올라감
- e.g. 200KB 크기의 process가 100KB 를 실행 중이면,
    - 큰 페이지를 하나 가지고 있으면, page 전체를 가져와 200KB 전체를 transfer, allocate
    - 1byte page를 가지고 있으면, 100KB만 가져와 100KB만 transfer, allocate
- 페이지 크기가 작을수록 page fault 수가 늘어남
    - page fault 오버헤드 : 인터럽트, register 저장, page 교체, queueing, table 업데이트

## 3. TLB Reach

## 4. Inverted Page Tables

## 5. Program Structure

## 6. I/O Interlock and Page Locking