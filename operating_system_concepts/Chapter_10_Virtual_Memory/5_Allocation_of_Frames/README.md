# 5. Alocation of Frames

1. Minimum Number of Frames
2. Allocation Algorithms
3. Global versus Local Allocation
4. Non Uniform Memory Access

---

- 고정된 양의 free frame list를 가지고 있을 때 어떻게 frame을 할당할 것인가?

#### simple case

- 총 128 frame
    - 35 frame : OS
    - 93 frame : user process
- demand paging을 사용
- 93 frame을 모두 free frame list에 둠
- user proces 가 시작되면 93번의 page fault가 일어나고,
- page-replacement 알고리즘에 의해 94번째 frame을 할당함
- process가 끝나면 93개의 frame이 free frame list에 들어감

#### 변형 : OS가 buffer, table space를 사용

- 3개의 free frame을 항상 넣어둠
- page fault 발생 시 buffer에서 가져올 수 있음

## 1. Minimum Number of Frames

- process가 실행되기 위해 필요한 최소한의 frame 수를 지정 (computer architecture에 의존)
- process에 할당된 frame 수가 적을수록
    - page fault 발생 빈도가 높아짐 -> process 실행 속도가 느려짐
- process당 최대 frame 수는 physical memory의 크기에 의존

## 2. Allocation Algorithms

## 3. Global versus Local Allocation

## 4. Non Uniform Memory Access