# 7. Summary

- 현대 appllication은 relational form 에 꼭맞는 데이터를 다루지 않음
    - 기존 single machine DBMS로 다룰 수 없는 양의 데이터
- internet of things (IOT) 가 생산하는 데이터가 많아짐
- BigData application을 위한 다양한 query language
    - 다양한 데이터 타입
- parallel storage, processing : 매우 크고 빠르게 scaling이 필요한 데이터
- Distributed data management system : file을 여러개의 machine에 저장
    - file system interface로 접근
- key-value storage system : key를 통해 value를 저장/접근
    - a.k.a. NoSQL system
    - 완전히 독립된 database system은 아님
- Parallel Distributed DB : data를 여러 machine에 저장하고 parallel로 연산
    - 기존의 DB interface 제공
- The MapReduce paradigm : parallel processing을 위한 programming model
    - `map()` : input data를 어떤 form으로 변환
    - `reduce()` : `map()`의 결과를 어떤 form으로 변환
- Hadoop system : MapReduce를 Java로 구현한 open-source system
- SQL을 통해 MapReduce를 사용할 수 있는 여러 application이 있음
- TODO. 책
- **streaming dat** : 계속해서 쿼리하는 데이터
    - 많은 application이 실시간으로 streaming data를 다룸
- Graph : 데이터를 그래프로 표현할 수 있게하는 데이터 타입