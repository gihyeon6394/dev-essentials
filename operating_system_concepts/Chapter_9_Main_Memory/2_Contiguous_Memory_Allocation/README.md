# 2. Contiguous Memory Allocation (연속 메모리 할당)

1. Memory Protection
2. Memory Allocation
3. Fragmentation

---

### memory의 2가지 파티션 = os + user process

- OS를 위한 memroy partition (low or high)
    - Linux, Windows OS는 높은 주소 사용
- user process를 위한 memory partition

### contiguous memory allocation (연속 메모리 할당)

- user process의 메모리 영역을 연속되게 할당
- 각 process는 메모리의 한 section을 차지하고 그 옆에 연속되게 다른 process의 section이 위치

## 1. Memory Protection

### relocation-register scheme

relocation register, limit register를 사용하여 메모리 보호

<img src="img.png"  width="60%"/>

- relocation register : 가장 작은 physical address를 저장
- limit register : process가 사용할 수 있는 logical address 범위를 저장
- MMU가 동적으로 logical address에서 relocation register를 더해 physical address를 만들어 메모리에 전달

### execution

- CPU가 실행할 process를 선택할 떄
- dispatcher 가 relocation, limit register를 load

### 메모리 영역의 유연성

- 메모리가 동적으로 변할 수 있게 함
- e.g. OS가 특정 device driver에 대해 코드와 buffer 공간을 가질 때,
    - device driver가 사용 중일 때만 memory에 load해둘 수 있음

## 2. Memory Allocation

### variable partitioning (가변 파티션)

<img src="img_1.png"  width="60%"/>

- 메모리를 여러 개의 partition으로 나누어 사용
- 각 영역은 하나의 process만 load 가능
- table : 각 partition의 상태를 저장
    - 초기에는 모든 메모리가 사용가능
- **hole** : 사용 가능한 메모리 주소 범위

### 작동

1. process가 memory를 요청
2. hole 집합 검사 : process가 들어가기에 충분한 hole 탐색
    - hole이 너무 크면 2개로 쪼갬 (1개는 hole로서 반납)
3. process 종료
    - memory 반납 (hole로서)
    - 인접한 hole이 있으면, merge

### dynamic storage-allocation problem (동적 저장 할당 문제)

- hole 집합에서 size _n_ 요청을 처리할 hole을 찾는 문제
- **first-fit, best-fit, worst-fit** 알고리즘
- **first-fit** : 맞는 크기 중 가장 처음 찾은 hole에 할당
    - hole 집합을 순차적으로 검사 (이전 first-fit 검사가 끝난 지점부터)
    - hole을 찾으면, process를 할당하고 종료
    - 일반적으로 가장 빠름
- **best-fit** : 맞는 크기 중 가장 작은 hole에 할당
    - hole 전체 검사 (크기 기준으로 정렬되어있지 않으면)
    - 잉여 hole 공간이 가장 작음
- **worst-fit** : 맞는 크기 중 가장 큰 hole에 할당
    - hole 전체 검사 (크기 기준으로 정렬되어있지 않으면)
    - 잉여 hole 공간이 가장 큼
- 시공간 효율 : first-fit, best-fit

## 3. Fragmentation

free memory space가 작은 조각으로 쪼개지는 문제 존재

### external fragmentation

- 전체 free meory space가 request를 받기에 충분하나,
    - 연속적이지 않을 때
    - 즉, hole이 여러 개로 쪼개져 있을 때
- first-fit, best-fit 모두 발생 가능
- **50-percent rule** : _N_ 개의 할당 block이 있을 때, 다른 0.5 _N_ 개의 block이 단편화로 손실

### internal fragmentation

- process가 필요한 메모리보다 더 큰 메모리를 할당받을 때
    - 사용하지 않는 메모리 공간
- e.g. 18,464 bytes의 공간에 18,462 bytles 할당 (2 bytes 낭비)

### 해결책 : compaction (압축)

- free memroy space를 하나의 큰 block으로 합치는 것
- 동적 relocation으로 excecution time 중에 완료 될때만 가능
- 비용
    - 간단한 방법 : 모든 process를 memroy 한쪽으로 몰고,
        - hole을 반대편에 몰아 합침
        - **매우 비쌈**

### 해결책 : _paging_

- process의 logical address 공간이 연속적이지 않도록 함
- memory 관리 기법 중 가장 일반적인 기술
