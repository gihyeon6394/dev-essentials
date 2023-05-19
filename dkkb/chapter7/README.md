# 도커 컴포즈를 익히자

- 편리한 도구
- 도커 설정을 기재한 설정 파일 이용
- 한번에 여러 개의 컨테이너 생성, 실행, 폐기
- 여러개의 컨테이너를 다룰 일이 많아졌으면 요긴

## 1. 도커 컴포즈란?

### 도커 컴포즈란?

- 시스템 구축과 관련된 명령어를 하나의 텍스트 파일에 기재
- 명령어 한번에 시스템 전체를 실행, 종료, 폐기 하도록 하는 도구
- 도커 엔진과 별개의 소프트웨어
- YAML 파일에 기재
    - 기재 내용 : 컨테이너나 볼륨을 어떠한 설정으로 만든다.
    - up command : 생성 / 시작
    - down command : 종료 / 폐기
- Dockerfile은 이미지를 만들기 위함이고, 컴포즈는 네트워크와 볼륨까지 만들 수 있다
    - **만드는 대상이 다름**

## 2. 도커 컴포즈의 설치와 사용법

### [실습] 도커 컴포즈 설치

- 리눅스는 직접 설치해야함
- 파이썬 3 런타임, 필요도구 (python3, python3-pip package)

### 도커 컴포즈의 사용법

- docker-compose.yml 정의 파일을 작성
- 도커 컴포즈가 docker-compose.yml을 읽어 도커엔진에 실행
- 정의 파일은 한 폴더에 하나만 가능
    - 정의파일이 여러개면 개수만큼 폴더를 만들어서 넣어놓으면 됨
- 서비스
    - 서비스가 모인것
    - 도커 컴포즈 용어

## 3. 도커 컴포즈 파일을 작성하는 법

### 도커 컴포즈 정의 파일의 내용 살펴보기

```yaml
version: "3"

services:
  apa000ex2:
    image: httpd
    ports:
      - "80:80"
    restart: always
```

### 컴포즈 파일(정의 파일)을 작성하는 방법

- YAML 형식 지키기
- 첫 줄에 도커 컴포즈 버전 기재
- 주 항목 services, networks, volumes 아래에 설정 내용 기재
- 항목 간의 상하 관계는 공백을 사용한 들여쓰기로 구분
- 들여쓰기는 같은 수의 배수만큼의 공백을 사용
- 이름은 주 항목 아래에 들여쓰기한 다음 기재
- 컨테이너 설정 내용은 이름 아래에 들여쓰기한 다음 기재
- 여러 항목을 기재하려면 줄 앞에 '-'를 붙인다
- 이름 뒤에는 콜론(:)을 붙인다
    - 콜론 뒤에는 반드시 공백이 와야한다 (바로 줄바꿈은 예외)
- \# 뒤의 내용은 주석으로 간주
- 문자열은 작은따옴표 또는 큰따옴표로 감싸 작성

### [실습] 컴포즈 파일 작성

1. 주 항목 작성
2. 이름 작성
3. MySQL 컨테이너 정의 작성
    - **의존 컨테이너를 먼저 작성한다**
4. 워드프레스 컨테이너 정의 작성

## 4. 도커 컴포즈 실행

### 도커 컴포즈 커맨드

~~~~
## 컨테이너 및 주변환경 생성
docker-compose -f [파일경로] up [옵션]

## 컨테이너 및 네트워크 삭제
## 볼륨과 이미지는 삭제되지 않음
docker-compose -f [파일경로] down [옵션]

## 컨테이너 종료
docker-compose -f [파일경로] stop [옵션]
~~~~

#### 주의사항 : 실제 생성된 컨테이너 파일과 정의파일에 명시한 이름은 다르다 

- 따라서 컴포즈로 생성한 뒤 ps 커맨드로 먼저 확인해라
- ex. penguin -> penguin_1

### [실습] 도커 컴포즈 실행

- **컴포즈 down 이후 이미지와 볼륨은 직접 삭제해야 함**
