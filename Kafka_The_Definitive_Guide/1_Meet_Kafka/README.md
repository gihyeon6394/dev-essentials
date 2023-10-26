# 1. Meet kafka

- Publish/Subscribe Messaging
- Enter Kafka
- Why Kafka?
- The Data Ecosystem
- Kafka's Origins
- Getting Started with Kafka

---

- 모든 application은 데이터를 만듦
    - 데이터 : log message, user activity, metrics, ...
- 데이터 이동이 쉬울수록 핵심 비즈니스에 집중 가능
- pipeline : data-driven enterprise의 핵심 컴포넌트

## Publish/Subscribe Messaging

#### _Publish/subscribe (pub/sub) messaging_

- sender (publisher) : data (message)를 receiver에게 다이렉트로 전송 안함
    - 메시지를 분류
- receiver (subscriber) : 특정 메시지 종류를 받음
- broker : 메시지가 발행되는 중앙 지점

### How It Starts (e.g. 모니터링 정보를 전송하는 application)

<img src="img.png"  width="70%"/>

- 간단한 message queue 혹은 프로세스간의 소통 채널

<img src="img_1.png"  width="70%"/>

- 모니터링하는 시스템을 추가하기 시작
- 모니터링 데이터를 요구하는 application이 많아짐
- application간의 **점대점 연결**이 많아짐 (복잡)

<img src="img_2.png"  width="70%"/>

- 모니터링 데이터를 받는 single application을 만들어서 모든 데이터를 받음
- 데이터를 요구하는 application에게 데이터 제공

### Individual Queue Systems

- pub/sub system 분리
- 점대점 연결 해소
- 중복이 증가 -> 하나의 중앙 시스템이 필요해짐

## Enter Kafka

## Why Kafka?

## The Data Ecosystem

## Kafka's Origins

## Getting Started with Kafka