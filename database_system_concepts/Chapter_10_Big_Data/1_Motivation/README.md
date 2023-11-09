# 1. Motivation

1. Sources and Uses of Big Data
2. Querying Big Data

---

- 1990에 들어 World Wide Web이 발전하며 single enterprise가 생산하던 데이터 이상의 데이터가 발생
    - 처음에는 web server의 text file에 저장하기 시작
    - text log file이 유용해지기 시작
        - 광고, 사용자 활동 추적, 마케팅 등
- 2000 년대에 social-media를 통해 나오는 데이터 폭증
- 이제 데이터는 전통적인 DB System에서 다루기 어려움
    - 데이터의 양이 너무 많음
    - 데이터의 형태가 다양함
    - 데이터의 속도가 빠름
    - 데이터 처리에 매우 높은 병렬성 필요
- 통칭 : Big Data

### Big Data vs Relational Data

- Volume
    - Relational Data (parallel relational DB) 보다 큼
    - parallel relational DB : 10 ~ 수백개 정도의 머신
    - Big Data는 수천개의 machine에 병렬 저장, 처리하길 원함
- Velocity
    - 이전보다 데이터 전송 속도가 빨라짐
    - 더 높은 속도로 DB 도 저장할 수 있어야함
    - 많은 app들이 DB가 저장되는 즉시 처리되기를 원함 (_streaming data systems_)
- Variety
    - relation of data, relational query language, relational database system
    - Big Data는 다양한 형태의 데이터를 다룸
- SQL query는 relational

### node

- cluster 에 있는 machine
- 매우 많은 machine으로 이루어진 cluster를 통해 Big Data 저장, 처리

## 1. Sources and Uses of Big Data

- 최초의 데이터 source : web server에서 발생하는 사용자 활동 log file
- 사용자가 많아짐에 따라 file이 커짐
- web log의 중요성도 커짐

### web log의 사용 목적

- 사이트의 어느 부분에 유저들이 더 많이 방문하는지
    - 데이터 기반으로 의사 결정
- 어떤 광고가 유저들에게 적합한지
    - 유저들의 광고 클릭
- 어떤 웹사이트의 구조가 유저들에게 친화적인지
    - 유저들이 자주 선택하는 페이지
- 페이지 조회수 기반의 유저 선호도
- **click-through** 정보 : 사용자가 광고를 클릭한 정보
    - 광고 효과를 측정
    - **conversion** : 광고를 클릭 후 실제 구매로 이어지는 것
    - click-through -> conversionl

### 고용량 데이터 출처

- 모바일 폰에서 생산한 데이터
    - app과 사용자가 상호작용한 정보
    - web site click
- retian 기업의 (온, 오프라인) 거래 데이터
    - Walmart와 같은 대형 소매업체
    - 이전에는 병렬 DB에서 처리
- 센서 데이터
    - High-end 장비들은 failure를 방지하기위해 health-check를 위한 여러 모니터링 장치를 둠
    - 탈것, 빌딩, 기계등에서 센서들이 서로 통신 (IoT)
- 네트워크 메타 데이터
    - traffic 모니터링
    - 음성 네트워크 통화 정보 등

## 2. Querying Big Data

- 다양한 형태의 데이터를 다루기 위해 SQL보다 더 넓고 다양한 쿼리가 필요함
    - 다양한 데이터 타입, 규모
- parallel storage, 데이터 처리
- transaction, 고성능

### 대용량/고속 DB system의 2가지 유형

#### 유형 1. 높은 확장성(scalability)이 필요한 트랜잭션 처리 시스템

- 매우 짧고 빈번한 query, update를 지원
- key-value store : 주로 관련 key와 데이터를 저장 key로 질의
    - e.g. key : 사용자식별자
- 예시 :  SNS에서 사용자 로그인 시, 사용자는 빠르게 친구들의 새로운 포스팅을 읽어야함
    - relational DB면 `join` 연산 필요
    - 대체 1. 각 사용자 별로 key-value sotre를 유지
        - value에는 사용자의 친구를 저장
        - 사용자 친구별로 각 포스팅을 질의
    - 대체 2. message queue
        - u0이 포스팅을 하면, 친구 ui에게 message를 전송

#### 유형 2. 높은 확장성과 non-relational data를 지원하는 쿼리 처리 시스템

- e.g. log 분석 시스템, document 저장, 웹에서 키워드 검색 지원
- 많은 application 에서 접근
- 요구 1. 대용량 파일을 저장할 수 있어야함
- 요구 2. 병렬 쿼리 지원
- 요구 3. 다양한 데이터 타입, 쿼리 프로그램 코드 지원 (관계형이 아님, non-SQL)

### Big Data 에 대한 별도의 처리방법 필요성

- 대용량의 text, image, video 데이터
- 이전에는 file system에 두고, stand-alone application으로 처리
    - e.g. 웹에서 키워드 검색 시 index 데이터 구조를 활용해 sql 질의
- stand-alone application으로 처리 역부족 (병렬 처리 필요해짐)