# Chapter 2. What Is Redis Used For?

- Identifying How Redis Can Help You
- Redis in the Real World

---

- Redis가 도움을 주는 분야
- 실시간 application에서 활용

## Identifying How Redis Can Help You

- redis use case
- benchmark 결과
    - 500,000 ops/sec (ms 이하의 latency를 보이며)
    - 50 million ops/sec (26 compute node에서)

### Real-time analytics

- 실시간 분석, 계산 e.g. top score, top-ranked, top posts, ...
- 게임, 옥션 마지막 입찰 (Redis Sorted Set)
- 추가로 sorting이 필요 없음
- Sorted Set은 순서를 유지하기 때문에, 순위를 알 수 있음 `ZRANGE`, `ZREVRANGE`
    - `ZADD`로 value를 넣을 때 순서를 함께 연산

## Fraud detection

- transaction에서 fraud detection
- shpping, financial transaction, ...

1. Redis Stream이 transaction 추적
2. Redis Bloom에 fraud 가능성 점수를 저장
3. RedisAI가 transaction에 대한 분석치를 제공
4. Redis TimeSeries가 trend를 분석

### Gaming and leaderboards

- 게임에서의 랭킹
- 데이터가 지역적으로 분산되어있음 (latency)

### Personalization with session management

- session 관리
- 사용자 입장에서 매우 빠르게 read가 일어나야하는 데이터
- Redis는 in-memory DB, Hash 자료구조이기 때문에, 빠른 read/write가 가능
- in-memory에도 즉작적인 failover가 가능

### Recommendation management (추천 시스템)

- 상품 추천 엔진
- 상품에 tagg를 붙여 추적
- e.g. 고객이 구매한 상품의 관련 상품 검색
    - `SINTER` : product 교집합 상품을 탐색
    - `SADD` : 각 product에 keyword를 tagg로 붙임

### Social apps

- social app은 실시간에 준하는 성능을 요구
- dis-based data store는 한계가 있음
- in-memory DB는 빠른 read/write를 제공
- caching, Pub/SUb pattern, Job/Queue 관리, Built-int 분석, Native JSON 관리

### Search

- indexing, querying, full-text search engine
- 다른 DB는 보조 인덱스를 사용해 검색을 수행
- RediSearch는 full-text search engine을 내장하고 있음
    - 다양안 언어간에 적용 가능
    - 인덱스 생성 속도 빠름

## Redis in the Real World

- e-commerce의 use case들
- 검색, 자동완성 등

### Cashing

- back-end app 간의 caching

### Large data sets

- native data type, in-memory 를 활용해 caching을 다룸
- Redis Enterprise Flash로 RAM 확장 가능

### Full-text fuzzy search

- RediSearch으로 searching 기술 확장
- 500 % 빠름 (stand-alone search-engine에 비해
- scoring, filtering, query
- Fuzzy Suggestion : 오타를 고려한 검색

### Geospatial and time series data

- native geospatial index, hash, sorted set, stream data type 활용
- 위치 기반 추천에 활용
- IoT 장치의 데이터 collection
    - location 기반으로 데이터를 지속적으로 생산

### Messaging/queuing

- 빠르게 움직이는 data를 다룸
- 수만가지 IoT 장치에서 생산되는 데이터 (native pub/sub mechanism)


