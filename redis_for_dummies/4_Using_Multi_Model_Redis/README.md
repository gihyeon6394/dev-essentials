# Chapter 4 Using Multi-Model Redis: Data Models, Structures, and Modules

- Redis Data Models
- Patterns and Data Structures
- Redis Modules

---

- Data Model : 데이터를 저장하는 방식
- Relational Model : 데이터를 table로 저장
- Redis는 Multi-Model DB
    - 다양한 데이터 모델을 지원
    - graph, document, key/value, full-text search 등

## Redis Data Models

- Redis의 데이터는 key로 식별
- key는 binary safe함 (image도 가능)
- key는 주로 string으로
- `SET` : 주어진 key에 대한 데이터 생성, 수정 (덮어쓰기)
- `GET` : 주어진 key에 대한 데이터 조회

### Strings and bitmaps

#### Strings

```bash
> SET user "kgh"
OK
> GET user
"kgh"
> SET logincount 1
OK
> INCR logincount
(integer) 2
> GET logincount
"2"
```

- `INCR` : 주어진 key에 대한 value를 1 증가
- 최대 사이즈 512MB (버전마다 다름)

#### bitmaps

- 1, 0으로 이루어진 배열
- boolean 표현시 유용
- `SETBIT` : 주어진 key에 대한 bit를 설정
- `GETBIT` : 주어진 key에 대한 bit를 조회
- `BITOP`, `BITFIELD` : bit 연산

### Lists (_arrays_)

- linked list로 구현됨 (쓰기 최적화)
- item의 원소 위치에 따라 읽기 성능이 다름`
    - 읽기 성능이 중요하면 Set을 사용`
- 1개의 key에 여러개의 value를 저장
    - `left`, `right`로 head, tail 저장/조회
- `LPUSH`, `RPUSH` : 주어진 key에 대한 value를 head, tail에 추가

```bash
> LPUSH users steve bob
(integer) 2
> LINDEX users 0
"bob"
> LINDEX users 1
"steve"
> LINDEX users 2
(nil)
> LRANGE users 0 -1
1) "bob"
2) "steve"
```

### Sets

- index로 접근 불가, 정렬되어있지 않음
- set의 member 존재여부 확인을 query
- key 중복 불가능

```bash
> SADD fruits apple
(integer) 1
> SMEMBERS fruits
1) "apple"
> SISMEMBER fruits apple
(integer) 1
> SISMEMBER fruits orange
(integer) 0
```

### Hashes

- key/value pair list
- 하나의 key에 여러 개의 field/value 저장

````
idolID : Karina001
name : Karina
age : 23
group : aespa
````

```bash
> HSET idolID:1 name Karina age 23 group Aespa
(integer) 3
> HGET idolID:1 name
"Karina"
````

### Sorted Sets

- set의 순서가 필요할 때 (leaderboard)
- single key에 여러 member(set 집합)을 저장

```bash
> ZADD idolAge 23 Karina 20 Minzi 30 IU
(integer) 3
> ZRANGE idolAge 0 -1
1) "Minzi"
2) "Karina"
3) "IU"
> ZRANGE idolAge 0 -1 WITHSCORES
1) "Minzi"
2) "20"
3) "Karina"
4) "23"
5) "IU"
6) "30"
> ZINCRBY idolAge 1 Karina
"24"
```

### HyperLogLogs

- unique한 item의 개수를 지속적으로 count
- e.g. 특정 웹사이트의 방문자수 카운트, 추적
- 내부적으로 hash를 유지해서 value가 있는지 확인, 있으면 db에 저장하지 않음l

```bash
> PFADD visitors 127.0.0.1
(integer) 1
> PFADD visitors 127.0.0.1
(integer) 0
```

## Patterns and Data Structures

### Pub/Sub

- 1개의 publisher : key-value 쌍을 publish
- 0개 이상의 subscriber : key-value 쌍을 subscribe

```bash
> PUBLISH channel1 "hello"
(integer) 1
```

```bash
> SUBSCRIBE channel1
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "channel1"
3) (integer) 1
1) "message"
2) "channel1"
3) "hello"
```

#### channels 계층, wildcard

````Bash
> PUBLISH channel1:Aespa "Black mamba!"
(integer) 1
````

````Bash
> SUBSCRIBE channel1:Aespa
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "channel1:Aespa"
3) (integer) 1
1) "message"
2) "channel1:Aespa"
3) "Black mamba!"
````

````Bash
> SUBSCRIBE channel1:*
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "channel1:*"
3) (integer) 1
1) "message"
2) "channel1:Aespa"
3) "Black mamba!"
````

### Geospatial Indexes

- 위치 정보 (위/경도)에 따라 데이터를 저장
- data간의 거리를 계산할 수 있음

```Bash
> GEOADD buildings -89.500 44.500 tower1
(integer) 1
> GEOADD buildings -88.500 44.500 tower2
(integer) 1
> GEODIST buildings tower1 tower2 
"79331.9227"
> GEODIST buildings tower1 tower2 mi
"49.2947"
```

### Redis Streams

- log 자료구조 : data가 logfile에 append하듯이 쌓이는 구조
- Redis Streams : 오직 쌓을수만 있고, FIFO로 읽을 수 밖에 없는 Stream
- `XADD` : stream에 데이터를 append

## Redis Modules

### RediSearch

- full-text search engine
- Redis에 document를 저장하고, document를 검색
- RediSearch 2.0부터 real-time secondary indexing 지원
- Boolean logic, 자동완성, fuzzy-logic 등에 쓰임

### RedisJSON

- JSON docs를 native format으로 저장
- 고속으로 JSON doc을 저장하고, 쿼리 가능
- e.g. 사용자 개인화 (상품 카타로그, 추천 등)

### RedisTimeSeries

- time-series data를 저장하고, 쿼리
- IoT, stock price, telemtry
- 내장된 Grafana, Prometheus, Telegraf 등과 연동, 시각화

### RedisGraph

- graph database : data를 graph 이론으로 표현
- e.g. SNS에서 relationship
- node들은 edge를 통해 다른 node와 연결

### RedisBloom

- 4가지 자료구조 이용 : Bllom filter, cuckoo filter, count-min sketch, top-k
- Bloom filter, cuckoo filter : item이 set에 존재하는지 확인
- count-min sketch, top-k : 자주 발생하는 item을 추적
    - count-min sketch : stream의 item의 빈도를 추적
    - top-k : 가장 자주 발생하는 item을 추적

### RedisAI

- Deep Learning, Machine Learning model 실행, 데이터 관리
- tensor 사용
- 음성인식, 자연어 처리, 객체 감지, 데이터 필터링 등

### RedisGears

- batch, event-driven data processing
- data를 가공할 function을 정의하고, Redis에 저장
- Python function, script 지원
- serverless 에서 유용