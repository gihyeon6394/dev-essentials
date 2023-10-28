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

## Selecting hardware

## Kafka in the Cloud

## Configuring Kafka Clusters

## Production Concerns

## Summary