# TCP / IP 응용 계층

- 응용 계층
- 웹서비스
- 파일 전송 서비스
- 원격 접속 서비스
- 메일 서비스

## 응용계층

<img src="img.png"  width="70%"/>

- 컴퓨터에서 웹사이트에 접속할 떄 필요한 프로토콜, 프로그램이 있는 계층
- 클라이언트의 요청을 전달하기 위해 서버가 이해할 수 있는 메시지로 변환하여 전송계층에 전달
- 응용 계층 프로토콜이 TCP, UDP를 통해 기능 수행
- 클라이언트는 TCP / UDP에 직접 접속 않고, 응용 계층을 통해 통신 서비스 사용
- 클라이언트 : 서비스 요청
- 서버 : 서비스 제공
- 클라이언트 < - > 서버 <sup>예시</sup>
    - 웹 브라우저 < - > 웹 서버 프로그램
    - FTP 클라이언트 < - > FTP 서버 프로그램
    - 메일 프로그램 < - > 메일 서버 프로그램

### 응용 계층 프로토콜의 종류

- TCP
    - HTTP : 웹 사이트 접속
    - FTP : 파일 전송
    - TELNET : 원격 접속
    - POP3 : 이메일 수신
    - SMTP : 이메일 발신
    - IMAP, ...
- UDP
    - DHCP, SNMP

### 포트번호 : 프로세스 구분

- 프로토콜 사용시 프로세스를 상호 구분하기 위한 포트번호 필요

<img src="img_1.png"  width="60%"/>

## 웹 서비스

- World Wide Web <sup>WWW</sup> 개발
- 웹은 TCP / IP에 의해 동작
- 응용계층 HTTP 프로토콜 사용
- 프로토콜 리소스 대상은 HTML <sup>Hyper Text Markup Language</sup> 문서

### HTTP 프로토콜

<img src="img_2.png"  width="50%"/>

- 클라이언트가 사이트 정보를 요청
- 서버가 정보를 응답
- URL : 요청하고자하는 리소스의 위치 주소
- MIME <sup>Multipurpose Internet Mail Extensions</sup> 정보

#### HTTP 특징

- Stateless <sup>무상태</sup> + Connectionless <sup>비연결성</sup>
    - 모든 요청과 응답은 이전 http 프로토콜과 독립적
    - 요청에 필요한 모든 정보를 매번 보냄
    - 무상태 단점을 보완하기 위해 **쿠키, 세션** 사용
- 1 : N 서비스에 적합

#### HTTP 메시지

- 요청 메시지와 응담 메시지로 구분

##### 요청 메시지

<img src="img_3.png"  width="60%"/>

##### 응답 메시지

<img src="img_4.png"  width="60%"/>

### HTTPS 프로토콜

- HTTP + SSL <sup>Secure Socket Layer</sup> / TLS <sup>Transport Layer Security</sup>
- HTTP 의 보안 강화 버전
- SSL 이나 TLS 프로토콜을 통해 세션 데이터 암호화
- 기본 포트 443

#### HTTPS 보안키

- 대칭키
    - 암호화와 복호화에 같은 키 사용
    - 클라이언트와 서버가 공유키를 가짐
- 공개키
    - 공개키와 개인키를 함께 사용
    - 암호화할 때 공개키, 복호화할 때 개인기
- SSL <sup>Secure Socket Layer</sup>
    - 인증서 필요
    - 인증서에 서비스 정보, 서버 측 공개키 포함
- CA <sup>Certificate Authority</sup> : 인증서 발급 기관
    - 브라우저 <sup>클라이언트</sup>가 CA 리스트를 보유
    - CA 존재 확인 후 CA 공개키를 이용해 인증서 복호화

### HTTPS 동작 방식

- 대칭키, 공개키 둘 다 사용
- 대칭키 : 데이터 전송
- 공개키 : 대칭키를 안전하게 전달

<img src="img_5.png"  width="60%"/>

*출처 : https://docs.oracle.com/cd/B14099_19/core.1012/b13995/ssl_intro.htm

1. Client Sends Hello Message : 암호화 알고리즘, SSL 버전 등 정보 전송
2. Server Sends Hello Message : 암호화 알고리즘, SSL 버전 등 정보 전송
3. Server Sends Certificate : CA 로부터 발급받은 인증서 전송
4. Authenticate Server : CA 리스트에서 인증서 확인
5. Generate random value : pre-master secret 생성
    - pre-master secret : 대칭키 생성에 사용되는 임시 키
    - pre-master secret 를 서버 측 공개키로 암호화하여 전송
6. Decrypts to retrieve pre-master secret : 서버 측 개인키로 pre-master secret 복호화
7. Calculates keys : 클라이언트와 서버 각자 SSL 세션에서 사용할 키 생성
8. Send Finished Message : Finish 메시지를 서로 전송
9. SSL handshake 종료, 데이터 전송

## 파일 전송 서비스 <sup>FTP, SFTP</sup>

- 클라이언트와 서버 간의 파일 전송을 위한 서비스
- FTP <sup>File Transfer Protocol</sup> : TCP / IP 기반
- HTTP 프로토콜로 인해 FTP 사용 줄어드는 추세
    - HTTP MIME 타입으로 파일 전송 가능
    - 클라우드, 웹 하드

### FTP 프로토콜 <sup>File Transfer Protocol</sup>

<img src="img_6.png"  width="40%"/>

#### 능동모드 <sup>Active Mode</sup>

- 서버가 클라이언트에 접속 시도
- 클라이언트 방화벽, 환경 조건에 따라 FTP 에러 발생 가능성

<img src="img_8.png"  width="30%"/>

1. 클라이언트 :  FTP 서버의 21 포트로 접속한 후 사용할 두번째 포트를 서버에 알려줌
2. 서버 : ACK 응답 and 클라이언트가 알려준 포트로 접속
3. 클라이언트 : ACK 응답

#### 수동모드 <sup>Passive Mode</sup>

- 1024 ~ 65535 포트 중 하나를 랜덤 비특권 사용

<img src="img_9.png"  width="40%"/>

1. 클라이언트 : FTP 서버 21 포트에 접속
2. 서버 : 클라이언트가 사용할 두번째 포트 알려줌
3. 클라이언트 : 서버가 알려준 포트로 접속
4. 서버 : ACK 응답

### SFTP 프로토콜 <sup>SSH File Transfer Protocol</sup>

- 신뢰할 수 있는 데이터 흐름을 통해 파일 전송
- 연결이 안정적 : 하나의 연결만 필요
- SSH 키 유효성검사, 관리 필요
- 소프트웨어간 호환성 문제

## 원격 접속 서비스

- 클라이언트에서 사용 권한을 가진 서버 시스템에 접속하여 작업을 수행
- e.g. TELNET, SSH

### TELNET 프로토콜

- 원격지의 컴퓨터를 이용하는 가상 단말 기능을 구현한 프로토콜
- 클라이언트 일부기능이 추가되어 소프트웨어로도 사용
- 모든 플랫폼 사용 가능
- 기본 포트 23
- 사용자 아이디 / 패스워드 필요

### TELNET 동작 방식

1. 클라이언트 : 원격 로그인을 통해 서버시스템에 TCP 연결 (포트 23)
2. 서버 : 시스템에게 가상의 터미널 제공
3. 클라이언트  : 터미널처럼 서버 시스템에 명령어 실행
4. 서버 : 명령을 수행한 결과를 클라이언트에게 전송

### SSH 프로토콜

- 기존의 유닉스 시스템 shell이 보안에 취약
- shell 에 암호화가 추가된 버전
- 강력한 인증방법
- 안전하지 않은 네트워크를 안전하게 통신
- rsh, rcp, rloign, telnet, ftp 등의 서비스를 안전하게 사용하도록 제공

### SSH 동작 방식

<img src="img_10.png"  width="40%"/> <img src="img_11.png"  width="40%"/>

## 메일 서비스

- 메일 주소를 이용하여 인터넷을 통해 이메일을 주고받을 수 있는 서비스
- TCP / IP 이전에는 대형 컴퓨터 <sup>Main Frame</sup> 에서 사용
- 사용자마다 편지함이 있음
- 같은 메인프레임에서만 사용 가능

### 메일 관련 프로토콜

- 이메일 발신 / 수신 을 가능하게 하는 프로토콜
- 크게 3가지
- SMTP : 메일 발신
- POP3 : 메일 수신
- IMAP : 메일 수신 시에 사용할 수 있는 프로토콜
    - POP3의 비동기성을 보완

## 참고

- https://ko.wikipedia.org/wiki/TCP/UDP%EC%9D%98_%ED%8F%AC%ED%8A%B8_%EB%AA%A9%EB%A1%9D
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages
- https://www.ssl.com/article/ssl-tls-handshake-overview/
- https://docs.oracle.com/cd/B14099_19/core.1012/b13995/ssl_intro.htm
