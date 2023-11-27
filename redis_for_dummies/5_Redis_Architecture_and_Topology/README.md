# 5. Redis Architecture and Topology

- Understanding Clustering and High Availability
- Examining Traditions and Durability

---

- prodduction level의 Redis
- Redis Enterprise HA

## Understanding Clustering and High Availability

- production level의 Database performance : clustering, sharding
- _shard_ : 특 DB의 일부분, Data를 여러 server에 분산하여 저장
    - 각 server는 shard 된 데이터에 대한 책임 소유
    - 부하를 각 serveer에 분산함

### Redis Enterprise cluster architecture

- clustering 을 내재하고있음
- 선형 scale-out 가능
- 각 cluster를 node로 간주
    - 각 node는 primary (master), secondary (replica)로 구성

#### Redis Enterprise cluster 구성 컴포넌트

- Redis Shard : data가 저장되고 관리되는 layer
    - open-source Redis의 single instance
- Zero-Latency Proxy : node 들이 사용하는 proxy
    - client와 node간의 stateless, multi-threaded communication
- Cluster Manager : cluster 관리
    - health, monitoring, rebalancing, resharding, provisioning, de-provisioning
- REST API : cluster 관리를 위한 REST API 제공ㅣ

### High availability

- network 분할이 있는 경우 3개의 replica를 운영
- Open-source Redis는 RAM을 계속 확장해나가야하므로, 비용, 구현 복잡해짐
- Redis 가 99.999 % uptime을 보장하는 방법
- Redis Enterprise는 in-memory master-slave replication을 사용

#### monitoring

- node level monitoring : noede를 모니터링
    - 문제 발생 시 shard failover process를 시작
- cluster level monitoring : cluster의 모든 node를 모니터링 (network 등)

### Running Redis at scale

- scaling vertically, horizontally 를 동시에 가능
- scaling up : server(cluster)의 capacity 여분이 있을 때
- scaling out : 더 많은 server를 배포하고, 데이터를 shard
- replica-of : https://redis.io/commands/replicaof/

### Redis on Flash

- Redis Enterprise, Redis Enterprise Cloud에서 가능
- RAM뿐만 아니라 분리된 flash memrooy에도 데이터를 저장할 수 있음 (SSD)
- 자주 사용되지 않는 value를 flash memory에 저장 (LRU 알고리즘으로 구현)
    - hot value : 자주 사용되는 데이터, RAM에 저장
    - cold value : 자주 사용되지 않는 데이터, flash memory에 저장

## Examining Traditions and Durability

### ACID (Atomicity, Consistency, Isolation, Durability)

- transaction의 특성
- Atomicity : 쓰기작업은 모두 성공하거나 모두 실패
    - 부분 쓰기 작업이 없음
- Consistency : transaction이 실행되기 전과 후의 상태가 일관되어야 함
    - e.g. 은행 계좌 이체
- Isolation : transaction이 실행되는 동안 다른 transaction으로부터 독립되어야함
- Durability : transaction이 성공적으로 완료되면, 그 결과는 영구적으로 반영되어야 함
    - e.g. 은행 계좌 이체가 성공적으로 완료되면, 그 결과는 영구적으로 반영되어야 함

#### Redis ACID

- Atomicity : Redis의 transaction 관련 명령어 `WATCH`, `MULTI`, `EXEC`
- Consistency : Redis가 제공하는 validation으로만 쓰기 허용
- Isolation : Redis는 single-threaded, 따라서 동시에 여러 transaction을 실행하지 않음
- Durability : Redis는 in-memory DB이지만 disk에 저장할 수 있음

### Durability

- Redis Enterprise는 fully durable data store

#### Append-only file (AOF)

- disk에 쓰기가 완료된 뒤 client에 응답
- 매 쓰기를 disk에 쓴후 응답하면 성능저하 발생
- Redis Enterprise는 AOF 성능을 최적화

#### Snapshot

- db 특정 시점의 snapshot을 disk에 저장
- durablity 확보를 위해 backup보다 자주 사용