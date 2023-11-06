# 3. Kafka Producers: Writing Messages to Kafka

- Producer Overview
- Constructing a Kafka Producer
- Sending a Message to Kafka
- Configuring Producers
- Serializers
- Partitions
- Headers
- Interceptors
- Quotas and Throttling
- Summary

---

- Kafka Producer를 사용하는 방법
- `KafkaProdcer`, `ProducerRecord` Object 생성방법
- record를 Kafka에 보내는 방법
- Kafka가 보내는 error 처리 방법
- Producer의 configuration
- Parition, 직려화 방법

> #### Third-Party Clients
>
> - Kafka는 binary protocol을 사용
> - Kafka network port로 올바른 byte sequence를 보내면 Kafka는 이를 처리 (읽고, 쓰기 가능)
> - 각 programming 언어에 Kafka wire protocol이 구현되어있음
> - Java, C++, Python, Go, ...

## Producer Overview

<img src="img.png"  width="70%"/>

1. `ProducerRecord`을 생성하는 거스로 message produce
    - 필수 : topic, value
    - 선택 : key, partition, timestamp, headers
2. producer가 message를 직렬화 (network에 보낼 준비)
3. partition을 명시 안했으면 partitioner가 partition을 선택
    - `ProducerRecord` key 기반으로 선택
    - partition이 선택되면 producer는 목적지 partition을 알게 됨
4. 목적지 (topic, partition)이 같은 message 묶음에 추가
5. 별도 thread에서 message 묶음을 알맞은 kafka broker에게 전송
6. Broker가 message를 받고 response
    - 성공적으로 쓰기가 완료되었으면 `RecordMetadata`를 반환
        - `RecordMetadata` : topic, partition, offset, timestamp
    - 실패했으면 return error
7. producer는 error를 처리하고, message를 재전송하거나, 다른 작업 수행

## Constructing a Kafka Producer

- Kafka 에게 message 보내는 방법 : Kafka producer 객체 생성하기

#### `bootstrap.servers` (필수값)

- `host:port` 쌍의 list
- producer가 Kafka cluster와 connection을 맺는데 사용
- 모든 broker를 포함할 필요 없음
    - 최소 2개 이상의 broker를 포함하는 것이 좋음 (fault tolerance)

#### `key.serializer` (필수값)

- `ProducerRecord`의 key를 byte array로 직렬화하는데 사용
- Kafka broker는 key와 message value를 byte array로 받음
- `org.apache.kafka.common.serialization.Serializer` 구현체를 사용
    - producer가 구현체를 사용해 직렬화
    - 직접 구현하지 않아도 `ByteArraySerializer`, `StringSerializer`, `IntegerSerializer` 등이 있음
    - `VoidSerializer` : key가 없는 경우 사용

#### `value.serializer` (필수값)

- `ProducerRecord`의 value를 byte array로 직렬화하는데 사용

#### 예시 : 필수 파라미터 기입

````
Properties propsKafka = new Properties();
propsKafka.put("bootstrap.servers", "broker1:9092,broker2:9092"); // broker list
propsKafka.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer"); // key serializer
propsKafka.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer"); // value serializer
````

#### 메시지 전송 방법 1. _Fire-and-Forget_

- 메시지를 server에 보내고 정상 도착 여부를 확인하지 않음
- error나 timout이 발생하면 알 수 없음

#### 메시지 전송 방법 2. _Synchronous Send_

- Kafka는 기술적으로 항상 async
- `send()`호출 시 return `Future`
- `Future`의 `get()`을 호출하면 blocking

#### 메시지 전송 방법 3. _Asynchronous Send_

- `send()` 호출 시 callback을 지정
- callback : Kafka broker로부터 응답이 오면 호출

## Sending a Message to Kafka

````
ProducerRecord<String, String> record =
    // 1.
    new ProducerRecord<>("CustomerCountry", "Precision Products","France");
try {
    // 2.
    producer.send(record);
} catch (Exception e) {
    // 3.
    e.printStackTrace();
}
````

1. `ProducerRecord` 객체 생성
    - 생성자에 topic 이름 (string), key, value
    - key, value는 각각 `key serializer`, `value serializer`에 의해 직렬화
2. `send()` : `ProducerRecord` 발송
    - message가 buffer에 저장된 후 별도의 thread에서 broker에게 전송
    - return `Future<RecordMetadata>`
    - message 전송 성공 여부 확인 안함
    - **production 에서는 적합하지 않음**
3. 여러 예외 발생 가능
    - `SerializationException` : 직렬화 실패
    - `BufferExhaustedException` : buffer가 꽉 참
    - `TimeoutException` : 발송 thread가 interrupt 됨ㅣ

### Sending a Message Synchronously

- kafka가 에러를 응답하면 예외를 catch
    - 에러 응답, 발송 재시도 횟수 초과 등
- trade-off : performance
    - Kafka broker의 상태에 따라 2ms ~ 수 초까지 걸림
- 응답할때까지 발송 thread는 대기
- **production 환경에서는 적합하지 않음**

````
ProducerRecord<String, String> record =
    new ProducerRecord<>("CustomerCountry", "Precision Products", "France");
try {
    // 1.
    producer.send(record).get();
} catch (Exception e) {
    // 2.
    e.printStackTrace();
}
````

1. `Future.get()` : blocking
    - record 전송이 실패하면 exception 발생
    - 성공하면 `RecordMetadata` 반환
2. 발송 도중 에러가 발생했으면 exception 발생

### Kafka 의 에러 타입 2가지 : _Retriable_ , _Non-Retriable_

- _Retriable_ : 재시도 가능
    - e.g. connnection error는 connection을 다시 맺으면 해결
    - `KafkaProucer`에 재시도 여부 설정 가능
- _Non-Retriable_ : 재시도 불가능
    - e.g. message size가 너무 크면 재시도해도 실패

### Sending a Message Asynchronously

- 대부분의 경우 Kafka의 응답이 필요 없음
- 메시지 발송 실패 여부는 알아야함 (error 파일 등)

````
// 1.
private class DemoProducerCallback implements Callback {
    @Override
    public void onCompletion(RecordMetadata recordMetadata, Exception e) {
        if (e != null) {
            // 2.
            e.printStackTrace();
        }
    }
}

// 3.
ProducerRecord<String, String> record =
    new ProducerRecord<>("CustomerCountry", "Biomedical Materials", "USA");
// 4.    
producer.send(record, new DemoProducerCallback()); 
````

1. `org.apache.kafka.clients.producer.Callback` 구현체 생성
2. `onCompletion()` : callback method
    - Kafka가 error 응답했으면 `e`가 nonnull
3. record 생성
4. `send()` : callback 전달

#### callback

- callback은 Producer의 main thread에서 실행됨
- 2개의 메시지를 서로 다른 partition에 보내도, callback은 main thread에서 순서대로 실행
- callback 안에서 blocking 연산 비추
- blocking 연산은 별도의 thread에서 concurrent하게 실행할 것

## Configuring Producers

- producer에 매우 많은 설정 파라미터들이 있음
- [Apache Kafka Docs](https://kafka.apache.org/documentation.html#producerconfigs) 참고
- 몇 설정은 memory, 성능, reliability에 영향을 미침

### client.id

- client (application) 에 대한 논리적 식별자
- 문자열
- Kafka broker가 어떤 client로부터 온 message인지 확인할 때 사용
- e.g. IP 104.27.155.134, Order Validation Service 등...

### acks

- producer가 write가 성공적으로 끝났다고 간주하기 위해
    - record를 수신해야하는 partition replicas의 수
- Default, leader가 record를 받으면 성공으로 간주 (since Kafka 3.0)
- 메시지 작성의 durability에 직결되는 설정

#### `acks=0`

- producer는 broker로부터 message 전송 성공 여부를 기다리지 않음
- message 전송 성공 여부를 알 수 없음
- message 유실 가능성
- message 전송 속도 빠름

#### `acks=1`

- leader replica가 message를 받으면 성공으로 간주
- leader replica에 message가 쓰이지 않으면, producer는 error를 받음
    - 이후 재시도 가능
- message 유실 가능성 : 최근 메시지가 새로운 leader에게 복제되기 전에 leader replica가 죽으면 유실

#### `acks=all`

- 모든 replica가 message를 받으면 성공으로 간주
- 가장 안전한 모드 : 하나 이상의 broker가 message를 받았다는 보장
- latency가 높음

> #### trade-off : producer latency vs. durability
>
> - `acks` 설정값이 낮을 수록 producer latency 낮음
> - `acks` 설정값이 높을 수록 durability 높음
> - _end-to-end latency_ : producer가 message를 보내고, consumer가 message를 받는데 걸리는 시간
> - `acks` 가 높을수록 end-to-end latency가 높음 (replica들에게 쓰이기 전까지 Kafka가 consumer에게 consume을 허용하지 않음)

## Serializers

## Partitions

## Headers

## Interceptors

## Quotas and Throttling

## Summary