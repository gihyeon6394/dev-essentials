<h1>컨테이너를 실행해 보자</h1>

- 컨테이너 생성, 삭제

1. 도커 엔진 시작하기/종료하기
   > 컨테이너를 실행중이지 않는다면 도커가 컴퓨터의 리소스를 거의 차지 안함  
   도커 엔진은 컴퓨터에 자동실행 설정이 가능하나, **컨테이너는 별도의 스크립트가 필요**

    - 도커 엔진을 시작/종료하는 방법
2. 컨테이너의 기본적인 사용 방법
    - 컨테이너 사용의 기본은 도커 명령어
      > ~~~~ 
      > ## docker 커맨드 대상
      > docker container run penguin
      > docker image pull penguin
      > docker container start penguin
      > 
      > ## docker 커맨드 (옵션) 대상 (인자)
      > docker container run -d penguin --mode=1
        > ~~~~
      > 
    - 기본적인 명령어 - 정리
    - [실습] 간단한 명령어를 사용해 보자
    - 대표적인 명령어
3. 컨테이너의 생성과 삭제, 실행, 정지
   > docker run = pull + create + start  
      stop 하지 않은 컨테이너 rm 불가능

    - docker run 커맨드와 docker stop, docker rm 커맨드
    - docker ps 커맨드
    - [실습] 컨테이너를 생성하고, 실행, 상태 확인, 종료, 삭제해 보자
4. 컨테이너의 통신
    - 아파치란?
    - 컨테이너와 통신하려면
    - [실습] 통신이 가능한 컨테이너 생성
5. 컨테이너 생성에 익숙해지기
    - 다양한 유형의 컨테이너
    - [실습] 아파치 컨테이너를 여러 개 실행하기
    - [실습] Nginx 컨테이너 실행하기
    - [실습] MySQL 컨테이너 실행하기
6. 이미지 삭제
    - 이미지 삭제
    - docker image rm 커맨드
    - docker image ls 커맨드
    - [실습] 이미지 삭제하기


