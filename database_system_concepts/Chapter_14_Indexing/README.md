<img src="img_1.png"  width="80%"/>

# CHPATGER 14. INDEXING

1. Basic Concepts
2. Ordered Indices
3. B+ Tree Index Files
4. B+ Tree Index Extensions
5. Hash Indices
6. Multiple-Key Access
7. Creation of Indices
8. Write-Optimized Index Structures
9. Bitmap Indices
10. Indexing of Spatial and Temporal Data
11. Summary

---

#### Index의 필요성

- 전체 record에서 일부만 질의하는 경우가 대부분
    - e.g. _전체 교수 중 물리학과 교수만 질의, 전체 학생 중 학번이 2010인 학생만 질의_
- 일부만 질의하는데 모든 record를 검색하는 것은 비효율적
- 질의 대상을 direct로 접근할 수 있도록 하는 자료구조

## 1. Basic Concepts

### Index 기본 컨셉

#### index는 책의 인덱스 (색인)과 같음

| 특징     | Database index | Book index   |
|--------|----------------|--------------|
| 목적     | record를 찾기 위함  | topic을 찾기 위함 |
| 형태     | ordered        | ordered      |
| 데이터 크기 | record의 일부     | 전체 책 내용의 일부  |

#### _student_ 를 _ID_ 로 조회 시 index를 사용하는 경우

1. index 조회
2. index를 통해 disk block을 찾음
3. disk block을 fetch

### 주의점

- index 자체가 매우 커지면 안됨
- index를 정렬해두면 탐색 시간이 줄어도, 전체 탐색시간이 여전히 길 수 있음
- add, remove 작업에 정렬된 index를 유지하기 위한 추가 작업이 필요함

### index의 종류

- ordered indices : 이미 정렬된 상태로 index를 유지
- Hash indices : value를 bucket 범위에 균등하게 분포, _hash function_ 을 기반으로 함
- Database applicaiton마다 적합한 index가 다름

### index 평가 기준

- **Access types** : access를 효율적으로 지원하기 위함
    - access : 특정 value나 range를 가진 record를 찾는 것
- **Access time** : access 시간을 위한 질의 기술
- **Insertion time** : insert하는데 걸리는 시간
    - insert 공간을 찾음 + index 구조를 유지하기 위한 추가 작업
- **Deletion time** : delete하는데 걸리는 시간
    - delete 대상을 찾음 + index 구조를 유지하기 위한 추가 작업
- **Space overhead** : index 구조를 위한 추가 공간

#### search key

- file의 record를 찾는데 사용되는 속성
- _primary key_, _candidate key_ , _superkey_ 와 다름

## 2. Ordered Indices

## 3. B+ Tree Index Files

## 4. B+ Tree Index Extensions

## 5. Hash Indices

## 6. Multiple-Key Access

## 7. Creation of Indices

## 8. Write-Optimized Index Structures

## 9. Bitmap Indices

## 10. Indexing of Spatial and Temporal Data

## 11. Summary
