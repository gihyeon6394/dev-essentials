# 8장. 통합점 : 게이트웨이, 터널, 릴레이

1. 게이트웨이
2. 프로토콜 게이트웨이
3. 리소스 게이트웨이
4. 애플리케이션 인터페이스와 웹 서비스
5. 터널
6. 릴레이
7. 추가정보

---

- HTTP로 여러 종류의 리소스 접근하기
- 다른 프로토콜, 애플리케이션과 HTTP로 통신하기
- 게이트 웨이 : 서로 다른 프로토콜, 애플리케이션 간의 HTTP 인터페이스
- 애플리케이션 인터페이스 : 서로 다른 웹 애플리케이션 간의 통신하는데 사용
- 터널 : HTTP 커넥션으로 HTTP가 아닌 트래픽 전송
- 릴레이 : 단순한 HTTP 프락시, 한번에 한개의 Hop에 데이터 전달

## 1. 게이트웨이

<img src="img.png"  width="70%"/>

- 모든 리소스를 하나의 애플리케이션으로 처리가 힘들어짐
- 게이트웨이 : 리소스를 받아 다른 서버로 라우팅해주는 역할
    - 리소스와 애플리케이션의 연결
- HTTP는 게이트웨이에 리소스 처리 요청
- 게이트웨이는 요청을 받아 응답을 보냄
    - 으답 : 동적인 컨텐츠, 데이터 베이스 질의 등

<img src="img_1.png"  width="50%"/>

- HTTP 요청에 대해 FTP 프로토콜을 사용하는 게이트웨이
- SSL(HTTPS) 요청에 대해 HTTP를 사용하는 게이트웨이
    - 보안 가속기 역할
    - 웹서버 바로 앞단에 위치
- 애플리케이션 서버 게이트웨이
    - HTTP 요청을 받아 애플리케이션 서버로 연결
    - e.g. 커머스 애플리케이션, 주식 거래 애플리케이션 등

### 1.1 클라이언트 측 게이트웨이와 서버 측 게이트웨이

````
<클라이언트 프로토콜>/<서버 프로토콜>

# 예시 (HTTP 클라이언트, FTP 서버)
HTTP/FTP
````

- server-side gateway `HTTP/*` : 클라이언트와 HTTP 통신, 서버와 외래 프로토콜 통신
    - HTTP로 들어오는 트래픽을 다른 프로토콜로 변환
- client-side gateway `*/HTTP` : 클라이언트와 외래 프로토콜 통신, 서버와 HTTP 통신
    - 외래 프로토콜로 들어오는 트래픽을 HTTP로 변환

## 2. 프로토콜 게이트웨이

![img_2.png](img_2.png)

1. 브라우저는 `gw1.joes-hardware.com`에 HTTP 요청
2. `gw1.joes-hardware.com`은 HTTP 요청을 받아 `ftp.irs.gov`에 FTP 요청
3. `ftp.irs.gov`은 FTP 요청을 받아 `gw1.joes-hardware.com`에 FTP 응답
4. `gw1.joes-hardware.com`은 FTP 응답을 받아 HTTP 응답
5. 브라우저는 HTTP 응답을 받아 화면에 표시

### 2.1 HTTP/*: 서버 측 게이트웨이

![img_3.png](img_3.png)

- `USER`, `PASS` 명령어를 사용해 FTP 서버에 로그인
- `CWD` 명령어를 사용해 디렉토리 변경
- 다운로드 형식을 ASCII로 설정
- `MDTM` 명령어를 사용해 파일의 최종 수정 시간을 확인
- `PASV` 명령어로 수동모드 진입
- `RETR` 명령어로 파일 요청
- FTP 커넥션이 맺어지면 게이트웨이로 FTP 응답을 보냄

### 2.2 HTTP/HTTPS: 서버 측 보안 게이트웨이

![img_4.png](img_4.png)

- 기업 내부로 들어오는 모든 웹 요청을 암호화
- 사용자는 HTTP로 탐요청하지만, 게이트웨이가 자도응로 모든 세션 암호화

### 2.3 HTTPS/HTTP: 클라이언트 측 보안 가속 게이트웨이

![img_5.png](img_5.png)

- 웹서버 앞 단에 위치해 인터셉트 게이트웨이, 리버스 프락시 역할
- HTTPS 트래픽을 받아 복호화하여 웹서버에 HTTP 요청
- 장점 : 암/복호화를 게이트웨이 하드웨어에서 처리 -> 원 서버 부하 감소
- 단점 : 네트워크 보안 안정성 검증 필요

## 3. 리소스 게이트웨이

![img_6.png](img_6.png)

- 리소스 게이트웨이 : web server가 application과 통신할 때 사용
- application server = destination server + gateway 를 하나의 서버에 통합
    - 클라이언트와 HTTP 통신
    - 서버측의 다른 backend application과 통신
    - API (Application Programming Interface) 제공
- CGI (Common Gateway Interface) : application gateway의 최초 API

![img_7.png](img_7.png)

### 3.1 공용 게이트웨이 인터페이스 (CGI)

- 웹 서버가 사용하는 표준 인터페이스 집합
- 특정 URL의 HTTP 요청에 따라 프로그램 실행
- 최초의 서버 확장이자 지금도 가장 널리쓰이는 확장방법
- 동적인 HTML, 신용카드 처리, DB query 등 실행
- 단점 : 성능 문제
    - 모든 CGI 요청마다 프로세스 생성
- Fast CGI : CGI와 유사하지만 데몬으로 동작하여 프로세스 생성, 제거

### 3.2 서버 확장 API

- HTTP 서버 자체의 성능이나 동작을 바꾸고 싶을 때 사용
- Apache는 API를 제공하여 개발자가 웹 서버의 동작을 바꿀 수 있게 해줌

## 4. 애플리케이션 인터페이스와 웹 서비스

- 웹 서비스 : 어플리케이션이 정보를 공유하는데 사용하는 메커니즘
    - HTTP 같은 표준 웹 기술 위에서 개발, 동작
- 더 복잡한 정보를 서로 교환하기에 HTTP header로는 제한적
- SOAP을 통해 XML 메시지를 주고받음
    - SOAP (Simple Object Access Protocol) : HTTP 메시지에 XML을 실어 보내는 방식의 표준

## 5. 터널

## 6. 릴레이

## 7. 추가정보

