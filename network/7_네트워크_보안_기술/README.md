# 네트워크 보안 기술

- 암호 기법
- 디지털 서명
- 방화벽
- 프락시 서버

## 암호 기법

### 암호화 <sup>Encryption</sup>

- 평문 <sup>Plaintext</sup>을 암호문 <sup>Ciphertext</sup>으로 변환하는 것
- 복호화 <sup>Decryption</sup> : 암호문을 평문으로 변환하는 것
- 키 <sup>Key</sup> : 암호화, 복호화에 사용되는 키
- 목적
    - 전달 과정의 기밀성 보장
    - 정보의 무결성 보장
    - 송신자, 수신자의 정당성 보장

### 암호화 기술의 분류

- 동작 형태에 따라
    - 대치 암호, 전치 암호, 혼합 암호, 대수화 암호
- 평문의 처리 방법에 따라
    - 스트림 암호화, 블록 암호화
- 암호화 키의 종류에 따라
    - 대칭 키 암호화, 공개 키 암호화

#### 대치 암호 <sup>Substitution Cipher, 치환 암호</sup>

- 메시지의 각 글자를 다른 글자로 대치
- 전통적인 방식

#### 전치 암호 <sup>Transposition Cipher, 전위 암호</sup>

- 평문의 글자를 재배열

#### 혼합 암호 <sup>Combination Cipher</sup>

- 대치와 전치 두 방법 모두 사용
- ex. LUCIFER, ENIGMA 등

#### 대수화 암호 <sup>Arithmetic Cipher</sup>

- 각 글자를 숫자로 바꾸어 수학적으로 처리
- ex. CRC <sup>Cyclic Redundancy Check</sup>, Vernam 암호 등

#### 스트림 암호화 <sup>Stream Cipher</sup>

- 평문과 같은 길이의 키 스트림을 생성하여
- 평문과 키를 비트단위로 합하여 암호화
- 평문은 한번에 한 비트씩, 랜덤하게 생성되는 키 스트림과 XOR 연산을 합하여 전송

#### 블록 암호화 <sup>Block Cipher</sup>

- 평문을 일정한 길이의 블록으로 나눈 뒤,
- 각 블록 단위의 암호화 과정을 수행하여 암호문을 생성
- 스트림 암호화 vs 블록 암호화
    - 스트림 암호화 : **비트** 단위 암호화
    - 블록 암호화 : **블록** 단위 암호화

#### 대칭 키 암호화 <sup>Symmetric Key Encryption</sup>

- 공통 키 암호화 <sup>Common Key Encryption</sup>, 비밀 키 암호화 <sup>Secret Key Encryption</sup>, 관용 암호화 <sup>Conventional
  Encryption</sup>
- 암호화 키 = 복호화 키
- 장점 : 구현 쉽고 실행속도 빠름
- 단점 : 키 분배, 관리 문제, 인증과 송수신 부인 방지 보장 되지 않음
- ex. AES, DES, IDEA 등

#### 공개 키 암호화 <sup>Public Key Encryption</sup>

- 암호화 키 = 공개키
- 복호화 키 = 개인키 <sup>Private Key</sup>
- 비대칭 키 암호화 <sup>Asymmetric Key Encryption</sup>
- 장점 : 키 분배, 관리 문제 해결, 인증과 송수신 부인 방지 보장 <sup>디지털 서명</sup>
- 단점 : 구현 어렵고, 처리 속도 느림
- ex. RSA, DSA 등

### 키 관리

- 사용되는 키를 안전하게 다루기 위함
- 키 생성, 키 분배, 키 저장, 키 소멸 등

#### 공개 키 관리

- 공개 키는 공개이므로 위/변조 가능
- 공개 키 인증서 <sup>cetificate</sup>
    - 공개 키를 인증하는 전자 증명 문서
    - 인증 만기일, 인증서 발급 기관 이름, 일련번호, 인증서 발급기관의 디지털 서명 포함
    - 인증서 형식 : ITU-T X.509 표준

### 공개키 암호화 방식

- Public Key Infrastructure <sup>PKI</sup> : 공개키 인증서를 발급하고 사용할 수 있는 인증서 관리 구조
- 인증기관 <sup>Certification Authority, CA</sup> : 공개키 인증서를 발급하는 기관
- 인증서
    - 기재된 통신 객체의 신분 보증
    - 인증기관의 디지털 서명이 포함
    - 인증기관의 개인키 없이 위/변조 불가능

## 디지털 서명 <sup>Digital Signature</sup>

- 네트워크 상에서 문서나 메시지 송/수신 시 사용
- 종이 문서에 사인이나 도장을 찍는 것을 디지털 문서화
- 공개 키 암호화 방식에서의 메시지 인증은 개인키를 이용한 메시지 작성자만 가능함
    - 이를 이용하여 메시지 작성자 본인을 인증하는 서명

### 디지털 서명의 인증

#### 서명 / 증명 알고리즘

1. 송신 : A, 수신 : B
2. A : A의 비밀키로 평문 암호화
3. B : A의 공개키로 암호문 복호화

#### 디지털 서명을 포함하는 메시지 전송

1. 송신 : A, 수신 : B
2. A : A의 비밀키로 평문 암호화
3. A : B의 공개키로 암호문 암호화
4. 네트워크 메시지 전송
5. B : B의 비밀키로 암호문 복호화
6. B : A의 공개키로 평문 복호화

#### 디지털 서명의 유효성

- 사용자 인증 <sup>Authentication</sup> : 디지털 서명자를 불특정 다수의 사람들이 검증 가능해야함
- 부인 불가 <sup>Non-repudiation</sup> : 디지털 서명자가 메시지를 부인할 수 없어야함
- 변경 불가 <sup>unalterable</sup> : 서명된 문서의 내용은 변경 불가
- 재사용 불가 <sup>non-reusable</sup> : 한 문서의 디지털 서명이 다른 문서의 디지털 서명으로 재사용 불가
- 위조 불가 <sup>unforgeable</sup> : 디지털 서명은 위조 불가

## 방화벽 <sup>Firewall</sup>

- 네트워크와 네트워크 사이에서 패킷을 검사하여 조건에 맞는 패킷만을 통과 <sup>packet filtering</sup> 시키는 소프트웨어나 하드웨어 총칭
- 방화벽은 각 계층마다의 패킷의 헤더 내용에 대한 확인항목 생성
- 확인 항목을 통과한 데이터만이 상위 계층으로 전달, 수상한 패킷은 버림
- 방화벽이 확인할 항목은 네트워크 관리자가 지정

### 방화벽의 종류

- 배스천 호스트 <sup>Bastion Host</sup>
- 스크리닝 라우터 <sup>Screening Router</sup>
- 이중 홈 게이트웨이 <sup>Dual-homed Gateway</sup>
- 스크린 호스트 게이트웨이 <sup>Screened Host Gateway</sup>
- 스크린 서브넷 <sup>Screened Subnet</sup>

#### 배스천 호스트 <sup>Bastion Host</sup>

- 방화벽 시스템 관리자가 중점 관리하는 세스템 중 하나
- 중요 기능 : 네트워크 접근 기록, 감사 추적을 위한 기록, 모니터링

#### 스크리닝 라우터 <sup>Screening Router</sup>

- 네트워크에서 사용되는 통신 프로토콜, 송수신지 주소, 통신 프로토콜의 제어필드, 포트번호 분석
- 내부 네트워크로 진입하는 패킷들의 허가, 거절 수행
- 내부 네트워크에서 외부 네트워크로 나가는 패킷의 허가, 거절 수행

#### 이중 홈 게이트웨이 <sup>Dual-homed Gateway</sup>

- 2개 이상의 네트워크에 동시에 접속된 배스천 호스트
- 하나의 네트워크 인터페이스는 인터넷 등 외부 네트워크에 연결되며,
- 다른 하나의 네트워크 인터페이스는 보호할 내부 네트워크에 연결되어
- 양쪽 네트워크 간의 직접적인 접근 허용 안함

#### 스크린 호스트 게이트웨이 <sup>Screened Host Gateway</sup>

- 스크리닝 라우터와 스크린 호스트를 혼합
- 내부 네트워크에 두어 스크리닝 라우터가 내부로 들어가는 모든 트래픽을 전부 스크린 호스트에게만 전달

#### 스크린 서브넷 <sup>Screened Subnet</sup>

- 외부 네트워크와 내부 네트워크 사이에 DMZ 역할을 하는 완충 지역 개념의 서브넷
- DMZ <sup>Demilitarized Zone</sup> : 외부 네트워크와 내부 네트워크 사이에 존재하는 완충 지역
- 스크리닝 라우터와 배스천 호스트 사용

## 프락시 서버

- 내부 네트워크에 있는 client를 대신하여 인터넷에 접속
- Client가 요청한 통신 서비스를 Client에게 제공해주는 서버
- 동작 절차
    1. Proxy server는 Client의 요청을 받음
    2. Proxy가 외부 서버에게 요청을 보냄
    3. 외부 서버가 응답을 보냄
    4. Proxy가 Client에게 응답을 보냄
- Proxy server의 장점
    - 안정성 : 사용자 인증 기능, 서비스 이용 제한 등의 기능을 Client에게 일괄 제공
    - 익명성 : Client의 IP 주소를 외부에 노출시키지 않음
    - 신속성 : Proxy server가 캐시 기능을 제공하여 외부 서버에 접속하지 않고도 Client에게 즉시 응답 제공 가능

### 프락시 서버의 종류

- HTTP Proxy : 웹 서비스 제공
- FTP Proxy : FTP 서비스 제공
- POP Proxy : POP 서비스 제공
- [참고 : HTTP 완벽 가이드 Proxy](../../http-the-definitive-guide/2부_HTTP_아키텍처/6장_프락시/README.md)