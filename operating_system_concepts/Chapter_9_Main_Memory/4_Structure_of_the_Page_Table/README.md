# 4. Structure of the Page Table

1. Hierarchical Paging
2. Hashed Page Tables
3. Inverted Page Tables
4. Oracle SPARC Architecture

---

- page table 구조 종류 : hierarchical paging, hashed page tables, inverted page tables

## 1. Hierarchical Paging (계층적 페이징)

- 현대 컴퓨터는 아주 큰 logical address space를 가짐 (2<sup>32</sup> ~ 2<sup>64</sup>)
- 따라서 page table도 매우 커짐
    - e.g. 32-bit logical address space, 4KB page size, 1 million entry page table
        - 페이지 수 = logical address space / page size = 2<sup>32</sup> / 2<sup>12</sup> = 2<sup>20</sup> = 1 million
        - 각 entry는 4 bytes일때, 4MB의 page table이 필요
- **해결책 : page table을 더 작은 조각으로 나눔**

### Two-Level Paging (이중 페이징)

<img src="img.png"  width="60%"/>

- page table을 더 작은 page로 나눔
- 32-bit logical address space, 4KB page size일 때,
- logical address 를 20-bit page number, 12-bit page offset으로 나눔
    - 10 bit page number, 10 bit page offset으로 한번 더 나눔

<img src="img_1.png"  width="50%"/>

- _p1_ : outer page table index
- _p2_ : inner page table index

### 주소 변환 방법

<img src="img_2.png"  width="70%"/>

- **forward-mapped** page table : 바깥에서 안쪽으로

### three-level paging (삼중 페이징)

<img src="img_3.png"  width="50%"/>

- 64-bit logcial address psace 에서는 두번의 page table을 사용해도 부족 (위 그림)
    - 2<sup>64</sup> / 2<sup>12</sup> = 2<sup>52</sup> 개의 page table entry 구성
    - 이중 페이징 적용 시, 2<sup>42</sup> 개의 outer page entry

<img src="img_4.png"  width="50%"/>

- outer page를 한번 더 나눔 (**three-level**)
- 2<sup>34</sup> bytes = 16GB의 page table이 필요
- 2nd outer page를 한번 더 나눌 수 있음 (**four-level**)
- 64-bit UltraSPARC architecture는 7-level paging을 사용

## 2. Hashed Page Tables (해시 테이블)

## 3. Inverted Page Tables (역 테이블)

## 4. Oracle SPARC Architecture (오라클 SPARC 아키텍쳐)
