# 2. Big Data Storage Systems

1. Distributed File Systems
2. Sharding
3. Key-Value Storage Systems
4. Parallel and Distributed Databases
5. Replication and Consistency

---

- Big Data를 다루는 application은 높은 확장성을 요구함
- 수백만 user, 많은 applcation이 동시에 접근
- 수천개의 computing, storage node에 걸쳐 partitioning되어 저장

### Big Data 저장소 유형

- distributed file system : 파일을 몇개의 machine에 걸쳐 저장
    - 파일에 접근은 전통적인 file-system interface를 통해
    - 큰 file을 저장할 때 사용 (e.g. log file)
- sharding across multiple databases :
    - _Sharding_ : record를 여러 시스템에 걸쳐 partitioning해서 저장
    - client software가 record를 저장할 DB를 결정
- key-value storage systems : 저장, 질의를 key를 통해
    - 일부 제한된 쿼리 기능 제공
    - 완전한 독립된 데이터베이스 시스템은 아님
    - NoSQL system이라고 불림 (SQL 지원 안함)
- Parallel and distributed databases : 인터페이스는 전통적인 DB
    - 데이터를 1개 이상의 machine에 걸쳐 저장
    - 쿼리 처리를 병렬로

## 1. Distributed File Systems

- 파일을 여러개의 machine에 걸쳐 저장
- client에게는 single file-system interface 제공
    - client는 파일의 저장위치를 알 필요 없음
- 적합 데이터 : 비구조화된 데이터
    - e.g. web page, web server logs, image, ...
    - 용량 : 수십 MB ~ 수백 GB 이상
- e.g. Google File System (GFS), Hadoop Distributed File System (HDFS)
- 파일은 1개 이상의 block에 걸쳐 저장
    - 1개 파일에 대한 block은 여러 machine에 걸칠 수 있음
    - 각 block은 여러 machine에 걸쳐 replicated 될 수 있음 (fault-tolerance)

#### File System의 기본 지원

- directory system : 파일을 계층형의 디렉토리로 저장 가능
- block의 식별자 번호와 file을 매핑
- 식별자로 data를 저장하고, 탐색
    - centralized file system에서는 식별자로 disk를 특정
    - distributed file system에서는 식별자로 machine + disk를 특정

### Distributed File System 예시 : Hadoop Distributed File System (HDFS)

<img src="img.png"  width="70%"/>

- **NameNode** : 파일의 metadata를 저장 (Hadoop의 코어)
    - file system의 모든 요청은 NameNode로 전달
    - 파일별 block 식별자 정보를 유지
    - block replication 정보를 유지
- **DataNode** : block을 저장
- Read : block이 있는 machine 식별자, file이 있는 block 식별자를 가져옴
- Write : block 식별자를 생성해서 각 machine에 할당
- API 지원
    - Java, Python 등의 언어로 구현된 API 제공

## 2. Sharding

- 대용량 파일, 수백만 이상 사용자, SNS application 등에서는 single application으로 역부족
- **sharding** : data를 partition 하여, 여러 DB (or machine)에 걸쳐 저장

### paritioning 방법

- _partitioning attributes (partitioning keys, shard keys)_ : data를 partitioning하는데 사용되는 attribute
    - 1개 이상의 속성 사용 e.g. 사용자 식별 키 등
- _range partitioning_ : partitioning attribute의 값의 범위에 따라 partitioning
    - e.g. 사용자 식별 키의 범위에 따라 partitioning
    - e.g. 1 ~ 100,000은 DB1, 100,001 ~ 200,000은 DB2, ...
- _hash partitioning_ : partitioning attribute의 hash 값에 따라 partitioning
    - e.g. 사용자 식별 키의 hash 값에 따라 partitioning
    - e.g. hash(1) % 10 = DB1, hash(2) % 10 = DB2, ...

### application에서 query 처리

- query를 처리하기 위해, client는 어떤 DB에 접근해야 하는지 알아야 함
    - paritioning attribute에 대한 정보를 알고 있어야 함
- 하나의 query를 여러 DB에 걸쳐 질의해야할 수도 있음

### 한계

- application이 paritioning 정보를 기반으로 query를 라우팅해야함
- 특정 DB에 부하가 생기면, 데이터를 다른 DB로 이동시켜야 함
- Key-Value Storage System이 일부 해결

## 3. Key-Value Storage Systems

## 4. Parallel and Distributed Databases

## 5. Replication and Consistency