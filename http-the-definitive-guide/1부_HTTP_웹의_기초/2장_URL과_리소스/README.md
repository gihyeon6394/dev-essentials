# 2장 URL과 리소스

- URL 문법, 여러 URL 컴포넌트가 어떤 의미를 가지며 무엇을 수행하는지
- 여러 웹 클라이언트가 지원하는 상대 UR가L과 확장 URL 같은 단축 URL
- URL 인코딩과 문자 규칙
- 여러 인터넷 정보 시스템에 적용되는 공통 URL 스킴
- URN을 포함한 URL의 미래

> - 인터넷 = 거대한 도시
> - 리소스 = 건물, 사람, 서비스 등
> - URL <sup>Uniform Resource Locator</sup> = 인터넷의 리소스를 가리키는 표준 이름

## 1. 인터넷의 리소스 탐색하기

- URL
    - 인터넷 상의 리소스 위치를 가리킴
    - 사람, 어플리케이션이 리소스를 찾고 사용하며 공유 가능
    - HTTP 및 다른 프로토콜을 통해 리소스에 접근 가능
    - URI의 부분집합

#### URL 구조

- 스킴://서버위치/경로 <sub>http://www.example.com/index.html</sub>
- 스킴 : 어떻게
- 서버위치 : 어디에
- 경로 : 무엇을

### 1.1 URL이 있기 전 암흑의 시대

스킴, 서버위치, 경로를 URL이 아니라 직접 말로 표현해야 했다.

## 2. URL 문법

- URL 문법은 스킴에 따라 **조금씩** 다름

#### URL 구조

- 스킴://사용자이름:비밀번호@호스트:포트/경로;파라미터?질의#프래그먼트
- ex. http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument

### 2.1 스킴 : 사용할 프로토콜

- 리소스에 어떻게 접근할 것인가

### 2.2 호스트와 포트

- 호스트 : 리소스를 호스팅하는 장비
- 포트 : 장비 내에서 리소스에 접근할 수 있는 서버의 위치
- 일반적으로 HTTP는 TCP 포트 80을 사용

### 2.3 사용자 이름과 비밀번호

- 많은 서버가 리소스에 접근을 위해 사용자 이름과 비밀번호를 요구
- FTP는 대부분 사용자 이름과 비밀번호를 요구

#### ex.

```http
 ftp://ftp.prep.ai.mit.edu/pub/gnu         
 ftp://anonymous@ftp.prep.ai.mit.edu/pub/gnu  
 ftp://anonymous:my_passwd@ftp.prep.ai.mit.edu/pub/gnu
```

### 2.4 경로

- 리소스가 서버 안에서 어디에 있는지 지정
- 계층적 파일 시스템과 유사

### 2.5 파라미터

- 프로토콜이 요구하는 정보
- key-value 쌍으로 구성, ';' 으로 구분
- ex. ...com/user;level=gold/index.html;type=d
    - level = gold, type = d

### 2.6 질의 문자열

- 리소스에 대한 추가 질의를 전달
- 게이트웨이 프로그램에 전달되는 정보
- key-value 쌍으로 구성, '?' 으로 구분 <sub>포맷상 제약은 아니나 일반적으로 사용</sub>
- ex. ...com/user;level=gold/index.html;type=d?key1=value1&key2=value2
    - key1 = value1, key2 = value2

### 2.7 프래그먼트

- 리소스의 일부분을 가리키는 이름
- ex. HTML 의 특정 paragraph
- URL의 # 문자 뒤에 위치
- 프래그먼트를 서버에 전달하지 않음
- ex. ...com/user;level=gold/index.html;type=d?key1=value1&key2=value2#SomewhereInTheDocument
    - SomewhereInTheDocument

## 3. 단축 URL

### 3.1 상대 URL

### 3.2 URL 확장

## 4. 안전하지 않은 문자

### 4.1 URL 문자 집합

### 4.2 인코딩 체계

### 4.3 문자 제한

### 4.4 좀 더 알아보기



3. 단축 URL
    1. 상대 URL
       > 기준 URL <sup>base</sup>을 기반으로 리소스의 상대적인 위치 지정   
       ex. ../images/logo.gif  
       장점
       > - 기준 URL이 변경되어도 상대 URL은 변경되지 않음
       > - 짧은 URL 길이
       > - 리소스의 서버 주소에 의존하지 않음

    2. URL 확장
       > 사용자 편의를 위함  
       호스트명 확장 : naver -> www.naver.com  
       히스토리 확장 : nav -> naver.com -> http://www.naver.com  
       **프락시 사용시 문제가 될 수 있음**

4. 안전하지 않은 문자
    1. URL 문자 집합
       > URL은 ASCII 문자 집합 + 이스케이프 문자 허용

    2. 인코딩 체계
       > 안전하지 않은 문자는 %로 시작, 16진수로 표현  
       ex. ...com/more%20<sup>빈문자</sup>info.html

    3. 문자 제한
       > 예약어 존재 <sub>ex. : ? . % 등등</sub>

    4. 좀 더 알아보기
       > 안전하지 않은 문자열을 애플리케이션 단<sup>브라우저</sup>에서 요청시에 변환해주는 것이 best

5. 스킴의 바다
   > 스킴 별 포맷들 TODO. tistory

6. 미래
   > URL에도 한계가 있음. URN이 언젠가는 쓰이겠지만 시간이 엄청 걸리게 될 것   
   PURL을 사용해서 URL을 URN처럼 사용 가능

    1. 지금이 아니면, 언제?
7. 추가 정보
   표
