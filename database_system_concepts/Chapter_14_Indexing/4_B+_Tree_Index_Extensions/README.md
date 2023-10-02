# 4. B+ Tree Index Extensions

1. B+-Tree File Organization
2. Secondary Indices and Record Relocation
3. Indexing Strings
4. Bulk Loading of B+-Trees Indices
5. B-Tree Index Files
6. Indexing on Flash Storage
7. Indexing in Main Memory

--- 

## 1. B+-Tree File Organization

<img src="img.png"  width="80%"/>

- index-sequentioal file의 degradation 문제 : file이 커지면, index의 크기가 커져, overflow block 발생
- overflow block을 해결하기 위해, B+-tree를 사용
- **B+-Tree File Organization** : B+-tree index 말고, 실제 file 구조도 B+-tree 구조로 구성 가능
    - leaf node에 실제 rocord 저장 (record pointer 저장 X)
    - half full

### insertion, deletion

- B+-tree index와 동일
- key value _v_ record insert 시
    - _v_ 보다 같거나 작은 key value를 가진 leaf node 중 가장 큰 node의 block에 insert
    - 공간이 충분하면 insert, 아니면 split
- delete 시
    - record를 가진 block을 제거
    - block _B_가 half full이 안되면, redistribution

### 공간 효율성을 유지하는 방법

- record를 저장하는 것은 pointer, key에 비해 많은 공간을 차지
- insertion, deletion
    - node가 차있으면, 이웃과 redistribution
    - 이웃 node도 차있으면, split
    - node의 적재량을 2n/3 유지 (n = node의 최대 적재량)

### 대용량 데이터 저장 e.g. clobk, blob

- disk block보다 큰 데이터를 저장 가능 (거의 수 gigabyte)
- 더 작은 reocrd로 순서대로 쪼개어 B+-tree에 저장
- record number : 쪼개진 reocord에 순서대로 체번, key로 활용

## 2. Secondary Indices and Record Relocation

- leaf node의 분할은 수십~수백개의 I/O operation 발생 가능
- split 후 레코드를 가리키는 보조 index (= leaf node) 들이 update 되어야함
    - record의 value 변경이 아닌, record의 위치 변경으로 인해
- 해결 방법
    - 보조 index 에 pointer 대신, primary index search key value를 저장
        - e.g. _dept_name_ 보조 index에 _ID_ 의 key value 목록을 저장
    - split으로 인한 relocation 필요 없음 (pointer가 아니기 때문)
    - access 비용이 높아짐
        - 보조 index -> primary index -> record (primary index를 거쳐야함)

## 3. Indexing Strings

## 4. Bulk Loading of B+-Trees Indices

## 5. B-Tree Index Files

## 6. Indexing on Flash Storage

## 7. Indexing in Main Memory
