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

## Sending a Message to Kafka

## Configuring Producers

## Serializers

## Partitions

## Headers

## Interceptors

## Quotas and Throttling

## Summary