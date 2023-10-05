# 7. Creation of Indices

### SQL 작성방법

- SQL 표준이 정의하지 않았으나, 대부분의 DB는 index DDL을 지원

````
-- create
create index <index-name > on <relation-name> (<attribute-list>);

-- drop
drop index <index-name>;

-- e.g.
create index group_idx on idol (group_name);
````

- `attribute-list` : searchkey를 형성할 relation의 attribute
- unique index
    - `attribute-list` 가 후보키인 경우, unique index 생성 가능
    - DB system에 따라 지원하는 index 타입이 다름
- user가 SQL query를 실행하면, index 사용 가능 query면 **자동으로** index 사용

### index 장점

- select, `join` 조건에 자주 사용되는 attribute
    - query 비용을 줄임
- e.g. `idol` relation에서 `group_name = AESPA` 인 record 질의 시
    - index가 없으면, `idol` relation의 모든 record를 읽고, `group_name` attribute를 검사해야 함 (많은 I/O 작업)
    - index가 있으면, index를 통해 record의 pointer를 얻어낸 후 몇번의 I/O 작업으로 record를 읽음

### index 단점

- relation에 수정이 있으면 index도 수정해야 함
- 많은 index를 생성하는 것은 update overhead를 증가시킴

### 실제 DBMS 구현

- relation에 PK가 있다면, 대부분의 DB system은 자동으로 PK에 대한 index를 생성
    - insert 시점마다 데이터 정합성일치여부를 검사해야함
    - index가 없다면 PK에 대한 검사를 위해 relation의 모든 record를 읽어야 함
- relation의 FK에 대해 대부분의 DB System은 index 미생성
    - 수동으로 index를 생성하는 것이 좋음
    - FK를 사용한 _join_ 연산이 잦음
- 몇몇의 cloud-based database의 경우 완전 자동 index 생성을 지원
    - DBA 간섭 없이
