# 6. Multiple-Key Access

1. Using Multiple Single-Key Indices
2. Indices on Multiple Keys
3. Covering Indices

---

- query에서 2개 이상의 attirubte로 구성된 index를 사용하여 질의하는 경우

## 1. Using Multiple Single-Key Indices

- _idol_ relation에 2가지 index 존재
    - _group_name_, _age_

```sql
select ID
from idol
where group_name = 'AESPA'
  and age = 23;
```

### 전략

- 전략 1 : _group_name_ index를 사용하여 _AESPA_ 인 tuple을 찾고, _age_ 를 검사
- 전략 2 : _age_ index를 사용하여 _23_ 인 tuple을 찾고, _group_name_ 을 검사
- 전략 3 : bitmap index 에서 효율적
    - _group_name_ index를 사용하여 _AESPA_ 인 record의 _pointer_ 찾음
    - _age_ index를 사용하여 _23_ 인 record의 _pointer_ 찾음
    - 두 pointer 집합의 **교집합**을 찾음

#### 전략 3은 다음을 모두 만족하면 성능이 별로

- _AESPA_ record 비율이 많음
- _23_ record 비율이 많음
- _AESPA_ 와 _23_ record의 교집합이 적음
- 위 세가지를 모두 만족하면, 많은 수의 pointer를 탐색하여 아주 적은 수의 record를 도출하게됨

## 2. Indices on Multiple Keys

- composite search key index 생성 (_group_name_, _age_)
- ordered B+-tree index 사용
- range query 가능
- 하나의 attribute로 조회 가능 (e.g. _group_name_)

```sql
select ID
from idol
where group_name = 'AESPA'
  and age = 23;

-- range query
select ID
from idol
where group_name = 'AESPA'
  and age > 20;

select ID
from idol
where group_name = 'AESPA';
```

### 단점 : 1개 이상의 비교 연산자

```sql
select ID
from idol
where group_name < 'AESPA'
  and age < 23;
```

- 조건에 맞는 record를 찾았으나, 각 record들이 서로 다른 disk block에 존재할 수 있음
    - 많은 I/O Operation 필요
- 대안 : _bitmap indices_ , _R-trees_ 같은 다른 index 구조 사용

## 3. Covering Indices

- record의 pointer와 함께 일부 attribute 의 value를 저장
- secondary index에서 유용
    - query 결과를 index 안에서 해결, 실제 record 조회 불필요
- e.g. nonclustering index _id_ 이 있을 때,
    - record pointer와 함께 _age_ attribute의 value를 저장해두면,
    - _age_ attribute를 조회하는 query에서 index만 조회하면 됨

### vs composite search key index

- (_id, age_) index를 사용할 수 있지만,
- covering index는 search key의 크기를 줄여,
    - nonleaf node에 대한 fanout을 늘림
    - index의 height을 줄임
