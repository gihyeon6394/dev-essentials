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

## 2. Sharding

## 3. Key-Value Storage Systems

## 4. Parallel and Distributed Databases

## 5. Replication and Consistency