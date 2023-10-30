# 2. Installing Kafka

- Environment Setup
- Installing a kafka Broker
- Configuring the Broker
- Selecting hardware
- Kafka in the Cloud
- Configuring Kafka Clusters
- Production Concerns
- Summary

---

- Apache Kafka broekr를 설치하는 방법
- Apache Zookeeper 셋업
    - Apache Zookeeper : Kafka가 broker의 메타데이터를 저장할때 사용
- 기본 설정 옵션
- broker를 실행할 하드웨어 선택
- 운영 환경에서 사용할 때 알아야하는 것들

## Environment Setup

### Choosing an Operating System

- Apache kafka 는 Java application
- 다양한 OS에서 구동 가능
    - Windows, Linux, macOS
- Linux를 가장 추천

### Installing Java

- Kafka, ZooKeeper는 OpenJDK Java에서 작동
- Oracle 사이트에서 다운로드

### Installing ZooKeeper

<img src="img.png"  width="70%"/>

- ZooKeeper : 중앙화된 서비스
    - 설정 정보, 이름, 동기화, 그룹 서비스 등 제공

#### Standalone server

1. `wget`으로 ZooKeeper 다운로드
2. `tar`로 압축 해제
3. `mv`로 압축 해제된 디렉토리를 `/usr/local/zookeeper`로 이동
4. `/var/lib/zookeeper` 디렉토리 생성
5. `zoo.cfg` 파일 작성
6. `JAVA_HOME` 환경변수 설정
7. `zkServer.sh` 실행

```Shell
# wget https://archive.apache.org/dist/zookeeper/zookeeper-3.5.9/apache-zookeeper-3.5.9-bin.tar.gz
# tar -zxf apache-zookeeper-3.5.9-bin.tar.gz
# mv apache-zookeeper-3.5.9-bin /usr/local/zookeeper
# mkdir -p /var/lib/zookeeper
# cp > /usr/local/zookeeper/conf/zoo.cfg << EOF
> tickTime=2000
> dataDir=/var/lib/zookeeper
> clientPort=2181
> EOF
# export JAVA_HOME=/usr/java/jdk-11.0.10
# /usr/local/zookeeper/bin/zkServer.sh start
JMX enabled by default
Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
#
```

#### ZooKeeper ensemble

- _ensemble_ : ZooKeeper 클러스터 (고가용성)
- 홀수 개수의 서버로 구성하기를 권장 (부하 분산 알고리즘 때문)
- 7개 이상 권장하지 않음
- 공통으로 설정하고, 각 서버마다 _myid_ 파일을 생성
    - _myid_ 파일 : 각 서버의 고유 ID를 저장하는 파일
    - e.g. zoo1.example.com, zoo2.example.com, zoo3.example.com

```Shell
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=20
syncLimit=5
server.1=zoo1.example.com:2888:3888
server.2=zoo2.example.com:2888:3888
server.3=zoo3.example.com:2888:3888
```

- `initLimit` : follower가 leader와 연결되기 위해 기다리는 시간
    - `tickTime`의 배수
    - 20 = 2000ms * 20 = 40 sec
- `syncLimit` : leader와 sync 되지 않은 follower들이 있을 수 있는 시간
    - `tickTime`의 배수
    - 5 = 2000ms * 5 = 10 sec

```Text
## ensemble 인스턴스 설정
server.X=hostname:peerPort:leaderPort 
````

- `X` : 서버 ID (숫자)
- `hostname` : 서버의 호스트 이름
- `peerPort` : ensemble 내에서 서버 간 통신에 사용되는 포트
- `leaderPort` : ensemble 내에서 leader 선출시 사용되는 포트
- client는 오로지 `clientPort`만 사용해서 ensemble에 접근
- single machine에서 테스트 (**운영환경에선 권장하지 않음**)
    - host 명은 localhost
    - 서버마다 포트만 다르게 부여 (`peerPort`, `leaderPort`)
    - 인스턴스마다 `zoo.cfg` 파일을 생성

## Installing a kafka Broker

- ZooKeeper가 설치되어 있어야 함

1. 설치
2. topic "test" 생성
3. topic "test"에 메시지 전송
4. topic "test"에서 메시지 읽기


1. 설치

```Shell
# wget https://archive.apache.org/dist/kafka/2.7.0/kafka_2.13-2.7.0.tgz
# tar -zxf kafka_2.13-2.7.0.tgz
# mv kafka_2.13-2.7.0 /usr/local/kafka
# mkdir /tmp/kafka-logs
# export JAVA_HOME=/usr/java/jdk-11.0.10
# /usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties

```

2. topic "test" 생성

```Shell
# /usr/local/kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 3 --topic test
Created topic "test".
# /usr/local/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic test
Topic: test     PartitionCount: 3       ReplicationFactor: 1    Configs: segment.bytes=1073741824
        Topic: test     Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: test     Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: test     Partition: 2    Leader: 0       Replicas: 0     Isr: 0

```

3. topic "test"에 메시지 전송

```Shell
# /usr/local/kafka/bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test
>Karina is Best   
>Karina is Aespa
>^C
```

4. topic "test"에서 메시지 읽기

```Shell
# /usr/local/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
Karina is Best
Karina is Aespa
Processed a total of 2 messages
^C
````

## Configuring the Broker

<img src="img_1.png"  width="60%"/>

### General Broker Parameters

- 단일 서버에 standalone broker 구축환경이 아니라면 적절히 설정 필요

#### broker.id

- 모든 borker는 고유한 ID (숫자)를 가져야함
    - 동일한 kafka cluster 내에서 unique
- default : `0`
- 추천 방법
    - host name이 `host1.example.com`이라면 `1`로 설정
    - host name이 `host2.example.com`이라면 `2`로 설정

#### listeners

- example 설정 파일에 TCP port `9092`로 설정되어있음
- `<protocol>://<hostname>:<port>` 형식으로 설정, 콤마로 구분
    - e.g. `PLAINTEXT://localhost:9092,SSL://:9091`
- port 번호가 1024 미만이라면 root 권한 필요
- Kafka를 root로 실행하는건 비추

#### zookeeper.connect

- ZooKeeper 포트
- `hostnanme` : ZooKeeper가 실행되는 호스트 이름
- `port` : ZooKeeper 클라이언트 포트
- `/path` : chroot 환경일 경우 설정

> #### Kafka Cluster는 Chroot 환경 권장
>
> - Kafka Cluster가 chroot 환경에서 실행되는 것을 권장
> - ZooKeeper ensemble이 다른 application과 충돌 없이 공유
> - 동일한 ensemble의 여러 ZooKeeper 서버 지정 : 서버 장애시 다른 ZooKeeper 서버로 연결 가능

#### log.dirs

- Kafka 는 모든 메시지를 disk에 씀
- `log.dirs` : Kafka가 log segment를 저장할 디렉터리
- 쉼표로 구분
- 여러 경로가 지정되면, 가장 적게 사용된 디렉터리에 저장
- 하나의 파티션의 log segment는 동일한 디렉터리에 저장

#### num.recovery.threads.per.data.dir

- log segment를 다루는 thread pool
- thread pool 사용
    - 시작 시 각 파티션의 log segment 열기
    - failure 후 시작 시 각 파티션의 log segment check and truncate
    - shut down시 log segment 닫기
- default로 각 log 디렉터리에 1개의 thread 사용
    - 시작/종료 시에만 사용됨
- 병렬 실행을 위해 thread 수를 늘릴 수 있음
    - `log.dirs` 가 3개, `num.recovery.threads.per.data.dir`가 4라면
        - 12개의 thread가 생성됨

#### auto.create.topics.enable

- default 로 자동으로 topic을 생성
    - 프로듀서가 토픽에 메시지를 작성할 때
    - 컨슈머가 토픽에서 메시지를 읽을 때
    - 클라이언트가 토픽의 메타데이터를 요청할 때
- `false`로 설정하면, 토픽을 명시적으로 생성하게 강제

#### auto.leader.rebalance.enable

- 하나의 borker에 모든 leader가 할당되는 것을 방지
- background thread가 `leader.imbalance.check.interval.seconds` 마다 실행

#### delete.topic.enable

- `false` : 토픽 삭제 불가
- topic을 임의로 삭제하는 것을 방지

### Topic Defaults

- 토픽마다 설정을 지정할 수 있음
    - parition 수, message retention 등
- 부가적인 관리 툴로 설정해야함

#### num.partitions

- 토픽 생성시 생성할 파티션 수
- default : `1`
- **토픽의 파티션수는 증가만 가능 (감소 불가)**
- 특정 topic이 `num.partition` 보다 작은 파티션이 필요한 경우 수동으로 topic 생성해야함
- 일반적으로 topic 의 partition 수 = cluster 내 broker 수 (e.g. 3개의 broker = 3개의 partition)
    - 혹은 borker 수의 배수 (e.g. 3개의 broker = 6개의 partition)
- partition 수 설정 예시
    - topic에서 1GBPS의 I/O처리를 원하고, consumer가 50MBps 성능일 경우 20개의 partition 필요
    - 50MBps * 20 = 1GBps
- 추천 방법 : partition 크기를 6GB 미만으로 제한해서 점차 확장

> #### How to Choose the Number of Partitions
>
> - topic에게 필요한 성능? write 100KB/sec, 1GB/sec?
> - 단일 partition에서 consumer의 최대 처리량
> - 단일 partition에 대한 producer 최대 처리량 (producer는 대게의 경우 consumer보다 빠르므로 skip해도 됨)
> - key 기반의 메시지 작성인 경우, 나중에 partition 추가는 매우 어려움
> - broker 별 공간 예측 (disk, network 대역폭)
> - 과소평가해야함 - broker가 메모리, 기타 리소스 사용 and 메타 데이터 업데이트, leadership 이전 시간 등이 추가됨
> - 미러링 데이터 환경에서 많은 partition은 bottleneck이 될 수 있음
> - VM, cloud 환경에서 IOPS 제한이 있을 경우 고려해야함

#### default.replication.factor

- `auto.create.topics.enable` 설정이 `true`일 경우, 토픽의 복제본 수
- 권장사항
    - `min.insync.replicas` 보다 1 이상 (하드웨어 설정이 매우 좋을 경우 2 이상)
- 일반적인 cluster
    - 최소 3개 이상의 partition 복제품이 있음
    - 각각 disk failure, broker failure, data center failure, rolling 배포 등을 대비

#### log.retention.ms

- message를 저장할 시간
- `log.retention.hours` 값이 default
    - `168` (7일)일
- `log.retention.minutes`, `log.retention.ms`로 설정 가능
- 추천 파라미터 `log.retention.ms`
- 중복으로 선언되어있으면 가장 작은 사이즈의 파라미터가 적용됨

> #### Retention by Time and Last Modified Times
>
> - disk의 각 log segment file이 마지막으로 수정된 시간 기반으로 삭제됨
> - e.g. log segment가 닫힌 시간 = file의 마지막 message timestamp

#### log.retention.bytes

- message 의 총 byte 수로 제한하는 방법
- partition마다 적용됨
- topic이 8개 partition을 가지고, `log.retention.bytes`가 1GB라면
    - topic에 최대 8GB의 데이터가 저장됨
- `-1` : 제한 없음

> #### Configuring Retention by Size and Time
>  `log.retention.bytes`, `log.retention.ms` 둘다 설정되어있는 경우
> 둘 중 하나가 충족되면 삭제됨
> e.g. `log.retention.bytes=1GB`, `log.retention.ms=1day`이면, 둘 중 하나가 충족되면 삭제됨

#### log.segment.bytes

- log retention 설정은 log segment에 적용됨 (message 단위 X)
- message가 publish되면 partition의 가장 최근 log segment에 저장
- default : `1GB`
- log segment 사이즈가 `log.segment.bytes` 에 도달하면, log segment가 닫히고, 새로운 log segment 오픈
- log segment가 너무 작으면 잦은 logsegment 열림/닫힘으로 전체적인 성능 저하
- topic의 produce 률이 낮을 경우 (예를 들어 하루에 100MB 생산)
    - default `log.segment.bytes=1GB`이기 때문에, 1개의 segment를 10일간 유지
    - `log.retention.ms=1week`인 경우, 최대 17일간 message가 보존됨

    1. log.segment.bytes=1GB`를 채우기 위해 10일간 유지되고 log segment가 닫힘
    2. `log.retention.ms=1week` 로 인해 닫히고 7일간 유지

> #### Retrieving Offsets by Timestamp
>
> - 특정 timestamp에서 partition offset 요청 시,
> - 해당 timesttamp에서 쓰기 중이던 log segment file을 찾음
> - 지정된 timestamp 이전에 생성된 file 중 가장 최근에 수정된 file을 찾음
> - 해당 file의 시작부분에 있는 offset 반환

#### log.roll.ms

#### min.insync.replicas

#### message.max.bytes

## Selecting hardware

## Kafka in the Cloud

## Configuring Kafka Clusters

## Production Concerns

## Summary