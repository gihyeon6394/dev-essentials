# 4. Page Replacement

1. Basic Page Replacement
2. FIFO Page Replacement
3. Optimal Page Replacement
4. LRU Page Replacement
5. LRU Approximation Page Replacement
6. Counting-Based Page Replacement
7. Page-Buffering Algorithms
8. Applications and Page Replacement

---

### over-allocating

- **over-allocating** : 실제 프로세스 frame 크기보다 더 multiprogramming 하는 것
    - 더 높은 CPU 활용도
- 총 frame 크기가 40일때,
    - 1개의 프로세스 크기가 10 page, 그 중 5개만 사용한다면,
    - multiprogramming 8개 가능

### memory-placement algorithm

- I/O, 프로그램 page에 얼마나 많은 메모리를 할당할 것인가
- I/O Buffer도 메모리를 사용

### page replacement

<img src="img.png"  width="80%"/>

- page fault 발생시,
    - 찾으려는 page가 secondary storage에 있으나, **free frame이 없다면,**
- 몇가지 방법
    - 방법 1. process 종료
    - 방법 2. **page replacement** (swapping 일종)

## 1. Basic Page Replacement

최근 frame 중 사용되지 않는 것과 교체하는 방법

<img src="img_1.png"  width="80%"/>

1. 요구한 page를 secondary storage에서 찾음
2. free frame 탐색
    1. free frame이 있다면 사용
    2. 없다면, page-replacement algorithm 실행 -> victim frame 결정
    3. page-out : victim freame을 secondary storage에 쓰고, 각 page table 수정 (valid -> invalid)
3. page-in :  요구한 page를 free frame에 쓰고, page table 수정 (invalid -> valid)
4. page fault가 발생한 process 재게

### modify bit (dirty bit)

- victim page를 page-out할 때 사용
- 각 page, frame에 modify bit를 둠
- page에 수정이 일어나면 하드웨어가 modfiy bit를 수정해 수정 여부 표시
- modify bit이 설정되어있으면 (= memory에 올린 후 수정이 일어난 적 있음)
    - page를 storage에 써야함
- modify bit이 설정되어있지 않으면 (= memory에 올린 후 수정이 일어난 적 없음)
    - page를 storage에 쓰지 않아도 됨

### frame-allocation algorithm

- multi-programming시, 각 process에 몇개의 frame을 할당할 것인가?

### page-replacement algorithm

- page replacement시, 어떤 page를 victim으로 할 것인가?
- OS마다 고유의 page-replacement algorithm을 가짐
- page-fault rate가 낮을수록 좋은 알고리즘

#### reference string

- reference string : 문자열 memory reference
- page-replacement algorithm을 테스트하기 위함
- 문자열, 난수 숫자 등의 페이지 참조를 연속으로 일으켜, page fault를 발생시키고 알고리즘을 테스트

## 2. FIFO Page Replacement

<img src="img_2.png"  width="80%"/>

- 가장 먼저 memory에 올라온 page를 victim으로 선택
- FIFO queue 사용
    - page가 memroy에 올라오면 enqueue
- 프로그램, 알고리즘이 단순
- 성능은 별로
- 사용중인 page가 교체되면
    - 즉시 page fault가 발동되어 다른 page를 교체
    - 사용중이었던 page는 다시 memory에 올라옴
    - page-fualt rate가 높음

### Belady's Anomaly

<img src="img_3.png"  width="80%"/>

- FIFO page-replacement algorithm이 더 많은 frame을 사용할수록 page fault가 늘어나는 현상
- `1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5` reference string 예시
- 4개의 frame을 사용할 때, page fault가 10번 발생
- 3개의 frame을 사용할 때, page fault가 9번 발생
    - frame이 많으면 많을수록 page fault가 더 많이 발생

## 3. Optimal Page Replacement

## 4. LRU Page Replacement

## 5. LRU Approximation Page Replacement

## 6. Counting-Based Page Replacement

## 7. Page-Buffering Algorithms

## 8. Applications and Page Replacement