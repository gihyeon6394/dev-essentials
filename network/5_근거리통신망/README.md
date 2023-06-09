# 근거리 통신망

- 근거리 통신망
- 근거리 통신망 분류
- 근거리 통신망의 모델
    - 유선 LAN
    - 무선 LAN
    - 고속 LAN
    - 초고속 무선 인터넷

## 근거리 통신망 <sup>Local Area Network, LAN</sup>

- 다수의 독립된 컴퓨터 기기들이 상호 통신 가능하도록 하는 데이터 통신 시스템 <sup>IEE 컴퓨터 표준위원회</sup>
- 좁은 지역 내에서 다양한 통신기기의 상호 연결을 가능케 하는 네트워크 <sup>William Stallings</sup>

> LAN의 4가지 특성
> 1. 단일기관 소유여야 한다.
> 2. 수마일 범위 이내의 지역으로 한정되어야 한다.
> 3. 어떠한 종류의 교환 기술을 사용해야 한다.
> 4. WAN과 비교하여 고속통신이 가능해야 한다.

### LAN의 특성

- **단일기관** 소유이다.
    - 행정/기술 적 제약없이 다양한 네트워크 구성 가능
- 광대역 전송매체의 사용으로 **고속통신** 가능
- 네트워크 내의 어떤 기기와도 전송 가능
    - 경우에 따라 우선순위 부여
- 패킷 지연 최소화
    - 통신거리가 제한
    - 고속 정보 전송
    - 버퍼 크기 줄임
- **라우팅 필요 없음**
    - 다른 네트워크에 연결되지 않기 때문에 별도의 전략이 필요 없음
- 낮은 오류율
    - 신뢰성
- **확장성과 재배치성**
- 종합적인 정보처리능력
    - 문자정보, 음성, 영상, 비디오 전송 가능

### LAN의 효과

- 정보자원 <sup>하드웨어/소프트웨어</sup> 공유
- 정보의 실시간 처리와 일관성
    - LAN에 연결된 모든 기기 온라인화
- 비용 절감
    - 기존에는 하나의 대형 컴퓨터에 대수의 단말기 연결
    - 새로운 컴퓨터 추가시 회선 추가 구축 필요
- 이기종 간의 통신
    - 서로 다른 노드들과 정보 교환
    - 프로토콜 변환 기능
- N:N 접속

### LAN 이용 분야

- 데이터 처리
    - 데이터 입력, 파일 전송, 질의/응답, 일괄처리, 트랜잭션
- 사무자동화
    - 문서처리, 이메일
- 공장 자동화
    - CAD/CAM, 재고관리, 장치 감독
- 원격 회의
    - 음성, 화상 정보 전송
    - 전화, 팩시밀리 등

## 근거리 통신망의 분류

#### 위상에 따라

- 성형
- 버스형
- 트리형
- 환형

#### 전송 매체에 따라

- 꼬임선 케이블 <sup>Twisted Pair Cable</sup>
- 동축 케이블 <sup>Coaxial Cable</sup>
- 광섬유 케이블 <sup>Optical Fiber Cable</sup>
- 무선 <sup>Wireless</sup>

#### 매체 접근 방식에 따라

- CSMA/CD
- Token Ring
- Token Bus

#### 전송 방식에 따라

- 베이스밴드 <sup>Baseband</sup>
- 브로드밴드 <sup>Broadband</sup>

### 성형 LAN <sup>Star</sup>

<img src="img.png"  width="30%"/>

- 중앙 제어기와 모든 노드가 점대점 연결
- 장점
    - 고장 발견 쉽고 유비보수 용이
    - 한노드의 고장이 전체 네트워크에 영향 적음
- 단점
    - 중앙제어기의 고장으로 인한 마비
    - LAN 초기 설치 비용

### 버스형 LAN

<img src="img_1.png"  width="30%"/>

- 버스 케이블에 모든 노드가 접속
- 장점
    - LAN 설치가 쉽고 구축 비용 낮음
    - 노드의 고장이 다른 네트워크에 영향 적음
- 단점
    - 베이스밴드 전송방식을 사용할 경우,
    - 전송거리가 멀어지면 신호 세기가 급격히 약해짐
    - repeater 필요

### 환형 LAN <sup>Ring</sup>

<img src="img_2.png"  width="30%"/>

- 각 노드가 양쪽 노드와 점대점 연결
- 신호는 보통 한 방향으로만 전송
- 장점
    - 설치, 재구송 쉽고 구축 비용 낮음
- 단점
    - 노드 추가 시 선로 절단 필요
    - 통신 제어 복잡

### 트리형 LAN

- 계층형 구조
- 성형 LAN의 변형
- 장단점이 성형 LAN과 유사
- 성형 LAN에 비해
    - 여러 노드 연결 가능
    - 노드간 전송거리 증가

### 베이스밴드 LAN

- 디지털 신호 직접 전송
    - 최대 1km 마다 리피터 필요
- 하나의 고속 <sup>10Mbps ~</sup> 채널 사용
    - 양방향 전송 가능
    - 시분할 다중화 방식 사용
- 꼬임선 케이블 or 동축 케이블 사용

### 브로드밴드 LAN

- 디지털 신호를 아날로그 신호로 변조하여 전송
- 단방향 전송 방식
    - 송신채널, 수신채널 별도 필요
- 주파수 분할 다중화
    - RF <sup>Radio Frequency</sup> 모뎀 사용
- 동축 케이블 or 광섬유 사용

### CSMA/CD

- Carrier Sense Multiple Access with Collision Detection
- 여러 통신 주체들이 동시에 통신하여 발생하는 충돌을 막기위한 프로토콜
- Ethernet에서 사용

#### 동작 방식

1. 노드는 데이터를 전송하기 전에 다른 기기가 통신 회선을 사용 중인지 점검
2. 통신회선이 사용중이라면 임의의 시간만큼 기다린 후 다시 확인
3. 사용중이지 않는 것이 확인되면 데이터 전송
4. 데이터 전송 중 충돌이 발생하면 통신 회선에 연결된 모든 노드에 jam 신호를 전송하여 충돌 사실 전파
5. 충돌이 발생하면 임의의 시간만큼 기다린 후 다시 전송

### Token Ring

- 환형 형태의 LAN을 구성한 뒤 토큰을 가진 노드만이 데이터 전송을 하게하는 제어방식
- IBM의 Ring-LAN

<img src="img_4.png"  width="60%"/>

#### 동작 방식

1. A 노드가 자유 토큰의 상태를 '사용 중'으로 바꿈
2. A 노드가 목적지 C, 송신지 A로 기록한 뒤 데이터를 토큰에 실어 B에게 전송
3. B 는 A가 전송한 토큰의 목적지 확인 후 자기 것이 아니므로 이웃 C에게 전송
4. C는 자신의 것이므로 데이터를 수신
5. C는 토큰을 수신 완료 상태로 변경하고 이웃 D에게 전송
6. D는 자신의 것이 아니므로 이웃 A에게 전송
7. A는 자신이 보냈던 토큰이 C에게 전송되었음을 확인
8. A는 자유토큰에서 데이터 제거 후 이웃 B에게 전송

### Token Bus

- Ethernet + Token Ring
- 물리적 구성은 버스형
- 논리적 구성은 토큰링
- Data Point 사의 ARCNET

<img src="img_3.png"  width="20%"/>

## 유선 LAN

- LAN 케이블로 모뎀을 연결하여 네트워크 구축
- 장점
    - 데이터 전송속도 높음
    - 통신 간섭 적고 안정적인 통신 가능
- 단점
    - 케이블 길이가 길어질 경우 손상 위험
    - 통신장애 발생할 가능성 높아짐
- 전송매체
    - 동축 케이블
    - 꼬임선 케이블
    - 광섬유 케이블
- Ethernet
    - 일반적으로 LAN 구성시 가장 많이 활용되는 기술 규격
    - 토큰 링 등을 대체함
    - OSI 모델에서 물리계층의 신호, 배선, 데이터 링크계층의 MAC 패킷, 프로토콜 정의
    - 속도가 점점 빨라짐 (이더넷 , 패스트 이더넷, 기가비트 이더넷)

## 무선 LAN

- 적외선이나 전파를 사용하여 네트워크 구축
- 장점
    - 이동 중에 통신 가능
    - 빠른 시간 내에 네트워크 구축 가능
    - 노드들의 배치도에 의존 X
    - 복잡한 배선 작업 제거
- 단점
    - 전송 속도가 느림
    - 간섭 가능성
    - 보안 취약
- 무선 LAN 전송 매체
    - 적외선 기술 사용
    - 대부분 2.4GHz ~ 60GHz 주파수 사용

### 무선 LAN의 통신 방식

#### Ad hoc

- "특별한 목적을 위해서"라는 뜻의 라틴어
- 무선 LAN 전파 범위 안에서 무선 LAN 카드를 장착한 노드끼리 직접 통신
- IBSS <sup>Independent Basic Service Set</sup> 라는 독립적 단위로 단독 네트워크 구성
- 다른 IBSS 노드와는 데이터 송수신 불가

#### infrastructure

- 무선 LAN 카드를 장착한 노드들이 허브나 라우터와 연결된 AP <sup>Access Point</sup> 를 통해 통신
- BSS <sup>Basic Service Set</sup>
    - 하나의 AP로 구성되는 무선 LAN
- ESS <sup>Extended Service Set</sup>
    - 서로 연결도니 BSS 들의 집합을 하나의 BSS 처럼 보이게 만드는 무선 LAN

## 고속 LAN

- 기존 LAN 프로토콜을 이용하면서 100Mbps 이상의 고속 통신을 지원하는 LAN
- e.g. Fast Ethernet, Gigabit Ethernet, FDDI

### Fast Ethernet

- 100Mbps의 고속 통신을 지원하는 LAN
- 기존 이더넷과 동일하나, 전송 가능한 케이블의 최대 길이를 줄여서 속도 향상
- 매체접근방식 : CSMA/CD

### Gigabit Ethernet

- 1Gbps의 고속 통신을 지원하는 LAN

### FDDI <sup>Fiber Distributed Data Interface</sup>

- 100 Mbps의 고속 통신을 지원하는 LAN
- ANSI 표준 -> ISO 표준
- 광섬유 케이블을 사용, 이중 링 구조의 LAN
- 2개의 링이 token passing 방식으로 운용

### Wibro <sup>Wireless Broadband</sup>

- 이동하면서도 고속의 무선 인터넷을 이용할 수 있는 시스템
- 최대 전송거리 1km, 최대 속도 25Mbps
- CDMA와 와이파이의 장점을 가짐
- 국내 휴대 인터넷 기술 표준으로 제안, IEEE 국제 표준으로 채택, WiMax 표준
- 우리나라 원천 기술
- 4G 이동 통신 기술

