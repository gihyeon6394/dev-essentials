# 6.IPC in Message-Passing Systems

1. Naming
2. Synchronization
3. Buffering

--- 

공유 메모리공간 없이 프로세스간 통신하고 동기화 함  
분산 환경에서 서로다른 컴퓨터가 네트워크로 연결되어있을 떄 용이 <sub>채팅 프로그램</sub>

#### message-passing 명령어

- `send(message)`
- `receive(message)`

#### 구현

- 메시지 사이즈는 유한, 무한 가능
- 메시지 사이즈가 유한이면, system-level에선 단순하나, application-level에서의 구현이 복잡
- 메시지 사이즈가 무한이면, system-level에서의 구현이 복잡

#### communication link

- M, Q 프로세스가 서로 통신하려면, 두 프로세스 사이에 communication link가 있어야 함
- comminication link의 논리적 구현 구조
    - Direct or indirect
    - Synchronous or asynchronous
    - Automatic or explicit buffering

## 1. Naming

- direct communication
    - hard-coding에 취약

### direct communication

- 송/수신자 프로세스의 이름을 명시
- 프로세스 간의 link는 자동으로 설정됨
- 링크는 두 프로세스 전용
- 프로세스간의 링크는 하나만 존재
- symmetric(대칭적) : 송신자와 수신자가 서로를 알고있음
    - `send(P, message)` : `P`에게 메시지 전송
    - `receive(Q, message)` : `Q`로부터 메시지 수신
- asymmetric(비대칭적) : 한쪽만 상대를 알고 있음
    - `send(P, message)` : `P`에게 메시지 전송
    - `receive(id, message)` : 메시지를 수신하고, 송신자 프로세스의 이름을 `id`에 저장

### indirect communication

- mailboxes or ports를 통해 메시지를 전달
- mailbox : 메시지를 read/write 하는 객체, 저장소
- `send(A, message)` : mailbox `A`에 메시지 전송
- `receive(A, message)` : mailbox `A`로부터 메시지 수신
- 프로세스 간의 링크는 공유된 mailbox가 있을 떄만 설정됨
- 하나의 링크는 둘 이상의 프로세스랑 연관 가능
- 프로세스 사이에는 여러 링크가 존재할 수 있으며 각 링크는 하나의 mailbox

#### 여러 프로세스가 하나의 mailbox를 사용하는 경우

P1, P2, P3가 mailbox A를 사용하는 경우,   
P1이 `send()`하고, P2, P3가 `receive()`한다면?  

- 해결방법
    - 링크에는 최대 2개의 프로세스만 연결되도록 제한
    - 동시에 최대 1개의 프로세스만 `receive()`할 수 있도록 제한
    - 시스템이 어떤 프로세스가 `receive()`할지 결정하도록 함 <sub>e.g. round-robin</sub>

#### mailbox의 owner

- process, OS가 owner가 될 수 있음
- process가 owner인 경우
    - mailbox의 충돌이 사라짐
    - process가 종료되면, mailbox도 함께 사라짐
- OS가 ownder인 경우
    - mailbox가 process로부터 독립적
    - process에게 다음 기능에 대한 메커니즘 제공
        - mailbox를 생성하고 삭제
        - mailbox를 통한 메시지 송/수신

## 2. Synchronization

- blocking, non-blocking
    - synchronous, asynchronous
- Blocking send : 송신자 프로세스는 메시지가 수신될 때까지 block됨
- Non-blocking send : 송신자 프로세스는 메시지를 송신하고, 자신의 작업을 계속함
- Blocking receive : 수신자 프로세스는 메시지가 수신될 때까지 block됨
- Non-blocking receive : 수신자 프로세스는 유효한 메시지나 null을 수신

### blocking send + blocking receive

- 생산자 소비자 문제 해결
- 송신자는 메시지가 수신될 때까지 기다리고,
- 수신자는 정상 메시지가 수신될 때까지 기다림

## 3. Buffering

- 메시지는 임시 큐에 저장되어 통신됨
- 큐 용량에 따른 분류
    - zero capacity : 0개의 메시지를 저장할 수 있음, 항상 blocking
    - bounded capacity : 유한개의 메시지를 저장할 수 있음, 큐 여유공간에 따라 blocking
    - unbounded capacity : 무한개의 메시지를 저장할 수 있음, 송신자는 항상 non-blocking
- zero capacity : 버퍼링이 없는 메시지 시스템
- bounded capacity : 자동 버퍼링이 있는 메시지 시스템
