# 여러 개의 컨테이너를 연동해 실행해보자

- 여러 개의 컨테이너를 동시 실행
- 컨테이너 끼리 통신하는 방법
- 컨테이너 간의 통신을 위한 네트워크를 만드는 법

## 1. 워드프레스 구축

워드프레스는 웹사이트를 만들기 위한 소프트웨어로 서버에 설치해 사용한다.  
컨테이너 간의 연결은 도커 가상 네트워크를 통해 한다.

### 워드프레스 사이트 구성 및 구축

### 도커 네트워크 생성/삭제

- **도커 가상 네트워크** : 컨테이너 들을 네트워크에 소속 시켜 컨테이너 간의 연결 구현

~~~~
docker network create (네트워크 이름)
docker network rm (네트워크 이름)
## 그 외 connect, disconnect, inspect, ls, prune, rm
~~~~

### MySQL 컨테이너 실행 시에 필요한 옵션과 인자

~~~~
docker run --name (컨테이너 이름) -dit --network (네트워크 이름) -e MYSQL_ROOT_PASSWORD=(패스워드) -e MYSQL_DATABASE=(데이터베이스 이름) -e MYSQL_USER=(사용자 이름) -e MYSQL_PASSWORD=(사용자 패스워드) mysql:5.7
~~~~

- -e : 환경 변수<sup>운영체제의 다양한 설정값 저장소</sup> 설정

### 워드프레스 컨테이너 실행 시 필요한 옵션과 인자

~~~~
docker run --name (컨테이너 이름) -dit --network (네트워크 이름) -e WORDPRESS_DB_HOST=(MySQL 컨테이너 이름) -e WORDPRESS_DB_NAME=(데이터베이스 이름) -e WORDPRESS_DB_USER=(사용자 이름) -e WORDPRESS_DB_PASSWORD=(사용자 패스워드) -p (호스트 포트):(컨테이너 포트) wordpress:latest
~~~~

## 2. 워드프레스 및 MySQL 컨테이너 생성과 연동

1. 네트워크 생성
2. MySQL 컨테이너 생성
3. 워드프레스 컨테이너 생성
4. 컨테이너, 네트워크 실행 확인
5. 뒷정리

### 이번 절의 실습 내용과 사용할 커맨드

### 워드프레스와 MySQL 컨테이너 생성 및 실행

## 3. 명령어를 직접 작성하자

- LAMP 스택 안에서 소프트웨어는 계속 바뀜 (apache -> nginx, mysql -> mariadb, php -> python, ruby, node.js)
- 도커에 능숙한 사람도 명령어를 작성할 떄는 옵션 및 인자를 참고한다.
- 커맨드 작성 시 인터넷에서 찾아서 조합하여 명령어를 작성할 줄 알아야 한다.

### 소프트웨어와 데이터베이스의 관계

### run 커맨드를 직접 작성하는 방법

## 4. 레드마인 및 MariaDB 컨테이너를 대상으로 연습하자

1. 네트워크 생성
2. MariaDB 컨테이너 생성
3. 레드마인 컨테이너 생성
4. 컨테이너, 네트워크 실행 확인
5. 뒷정리

### 레드마인 및 MySQL 컨테이너 생성

### 레드마인 및 MariaDB 컨테이너 만들기
