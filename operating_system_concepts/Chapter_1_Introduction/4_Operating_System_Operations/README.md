# 4. Operating-System Operations

1. Multiprogramming and Multitasking
2. Dual-Mode and Multimode Operation
3. Timer

---

OS 는 프로그램이 실행되는 환경을 제공한다.

### bootstrap program 과 Kernel

- 컴퓨터 부팅시 필요한 프로그램
    - CPU 레지스터, 장치 제어기, 메모리 초기화 등
- 하드웨어 내 펌웨어에 저장
- 부트스트랩 프로그램은 OS 커널을 메모리에 로딩

### 시스템 부팅

커널이 메모리에 올라가면, 서비스를 시스템과 사용자에게 제공 가능해진다.

- system daemon : 몇 서비스는 커널 외부에서 시스템 프로그램에 의해 제공
- systemd : 리눅스의 첫번쨰 프로그램으로서 많은 다른 데몬을 시작시킴

## 1. Multiprogramming and Multitasking

<img src="img.png"  width="40%"/>

Multiprogramming : 2개 이상의 프로그램을 구동하는 능력

### Multiprogramming increases CPU utilization

- 싱글 프로그래밍은 다른 CPU가 놀고 있어서 비효율적
- 유저들은 **동시에** 여러개의 프로그램이 구동되기를 원함
- **process** : 실행 중인 **프로세스**

### idea : idle 시간 최소화

1. OS가 몇몇의 프로세스들을 동시에 메모리에 로딩해둠
2. 실행할 프로세스를 골라 실행
3. I/O 대기와 같은 대기상태에 돌입하면,
    - 다른 프로세스를 실행하러 감
4. CPU가 유휴상태에 빠지지 않음

### Multitasking

- Multitasking : Multiprogramming 의 논리적 확장
- 여러 프로세스들을 스위칭 하면서 실행
- e.g. I/O 진행 시
    - display to user
    - touch screen
    - user keyboard, ...
    - 위 작업들을 기다릴바에야 다른 프로세스를 실행하러 감

### 추가 작업

- memory management : 메모리에 동시에 여러 프로세스를 올리려면 메모리 관리 필요
- CPU scheduling : 어떤 프로세스를 선택하여 실행할 것인가?
- virtual memory
    - 합리적인 응답시간을 제공하기 위해,
    - 프로세스의 실행을 완전히 메모리만을 이용하여 진행하지 않음
    - 실제 물리 메모리보다 큰 프로그램을 실행 가능
- storage management
- 프로세스 동기화 메커니즘 : deadlock 방지

## 2. Dual-Mode and Multimode Operation

#### 실행모드 구분

적절한 프로그램 실행을 위해 OS 코드에 의한 실행 <sup>1</sup> 과 사용자 코드에 의한 실행 <sup>2</sup>을 구분해야함.

### user mode, kernel mode <sup>also called supervisor mode, system mode, or privileged mode</sup>

- user mode : 사용자 어플리케이션 구동 시
- kernel mode : 컴퓨터의 제어권을 얻었을 때
- mode bit : 최근 모드가 무엇이었는지 가르킴
    - 0 : kernel, 1 : user
- 모드 비트를 통해 어떤 모드에 의해 실행된 작업인지 구분 가능

#### user mode에서 kernel mode로 전환

<img src="img_1.png"  width="60%"/>

- 사용자 어플레케이션 실행 시 user mode로 진행 되다가,
    - system call <sup>interrupt, trap </sup>을 통해 OS로부터 서비스를 요청하면 kernel mode로 전환
- 평시에는 user mode 임

1. kernel mode : 시스템 부팅 and OS 로딩
2. user mode 로 전환 : 사용자 어플리케이션 로드
3. trap이나 inturrupt 발생 시 kernel mode 로 전환

### 모드 구분의 이유

- 악성 사용자로부터 OS 보호
- privileged instructions
    - 커널 모드에서만 가능
    - 사용자 모드에서 특권 명령을 시도 시 잘못된 명령으로 간주하여 OS에게 전달
- 두가지 모드 이상도 존재함
    - e.g. Intel processors 의 protection rings

### system call

- 사용자 프로그램이 OS에게 본인을 대신하여 예약된 작업을 요청하는 것
- 모드비트가 kernel mode로 전환됨
- system-call 서비스는 OS의 일부가 됨
- 커널이 어떤 system call이 일어났는지 검사함
- 파라미터에 어떤유형의 사용자 요청이 들어왔는지 담김
- 파라미터를 통해 유효한지 확인하고 실행

## 3. Timer

사용자 프로그램이 무한 루프에 빠지거나, OS에게 제어를 반환하지 않는 상황을 막아야 함.  
따라서 Timer를 사용

- 일정 시간 후 컴퓨터에게 interrupt 발생시킴
- 시간은 가변적이거나 고정적일 수 있음
- e.g. 10 bit 카운터, 1 ms timer
    - 1ms 씩 1024 ms 까지 카운팅, 1ms 마다 interrupt 허용
- 운영체제는 타이머가 인터럽트를 발생시키도록 보장
- 타이머가 인터럽트 발생시키면 제어는 운영체제로 넘어옴

> Linux의 타이머
>
> - HZ : 인터럽트 주파수 지정
> - e.g. 250HZ : 1초에 250번 인터럽트