# 9. Building and Booting an Operating System

1. Operating-System Generation
2. System Boot

---

## 1. Operating-System Generation

### OS를 구성하는 순서

1. OS 소스코드 작성
2. OS를 구동할 환경 설정
3. OS 컴파일
4. OS 설치
5. 컴퓨터 부팅

### 운영체제 설정파일 사용 방법

운영체제 구성 중 주요 설정은 어떤 기능을 포함할지 정하는 것이다.  
일반적으로 설정파일에 매개변수로서 정의되어 있다.

- 방법 1. 설정 파일 직접 수정
    - 운영체제는 해당 소스코드로 컴파일 <sub>System build</sub>
    - 컴파일된 파일은 해당 시스템에 맞게 데이터 선언, 초기화 상수 구성
    - 주로 임베디드 시스템에서 사용
- 방법 2. 미리 컴파일된 객체 모듈들을 선택하여 운영체제 구성
    - 필요한 I/O 장치만 선택하여 OS에 연결
    - 시스템 재컴파일 필요 없어 시스템 생성 속도가 빠름
    - 다른 하드웨어 구성 지원 못할수도 있음
    - 대부분의 현대 운영체제 <sub>Linux, Windows</sub>에서 사용
- 방법 3. 완전 모듈화된 시스템 구성
    - 런타임 중 모듈을 추가하거나 제거할 수 있음
    - 시스템 구성 매개변수로 시스템 생성

### Linux 구축 방법

1. 리눅스 소스코드 다운로드
2. `make config` 로 커널 설정 : `.config` 파일 생성됨
3. `make` 로 커널 컴파일
    - `.config`에 설정된 매개변수들을 기반으로 컴파일
4. `make modules` 로 커널 모듈 컴파일
    - `.config`에 설정된 매개변수들을 기반으로 컴파일
5. `make modules_install` 로 커널 모듈을 `vmlinuz`에 설치
6. `make install` 로 커널을 시스템에 설치

#### Linux Virtual Machine

- Linux가 아닌 Host OS에서 Linux를 실행하는 방법
- ex. VirtualBox, VMWare

## 2. System Boot

- 하드웨어가 커널의 위치를 알고, load 하는 방법

### boot loader

- OS 구동 전 미리 실행하여 커널이 올바르게 시동되기 위한 작업을 수행, OS를 구동
- boot loader는 Windows, Linux, Android 등 모든 OS에 존재
- recovery mode나 single-user mode로 부팅할 수 있도록 지원 <sub>하드웨어를 진단하기 위함</sub>

### boot process

1. bootstrap program, boot loader가 커널에 위치
2. 커널이 1번을 메모리에 load하고 start
3. 커널이 하드웨어 초기화
4. root file system mount

- BIOS 기반 : 비휘발성 펌웨어에 위치한 작은 boot loader
    - boot block을 읽어 메모리에 load
    - boot block : 일반적으로 bootstrap program의 길이를 알고있음
- UEFI 기반 <sup>Unified Extensible Firmware Interface</sup>
    - BIOS의 대체제
    - 64bit 시스템과 더 용량이 큰 디스크를 지원
    - 단일로 완벽한 boot manager로서 다중 BIOS 부팅 포르세스보다 빠름

#### bootstrap

- 다양한 업무를 수행
- 커널 프로그램을 포함한 파일을 load
- 하드웨어의 상태를 진단 : 메모리, CPU, 장치 조사
- 조사가 끝나면 부팅 단계로 돌입
- CPU 레지스터, 장치 제어기, 메인메모리까지 모든 것을 초기화
- 그 후 OS를 작동하고 root file system을 mount
- GRUB : Linux의 오픈소스 bootstrap program

```shell
# GRUB /proc/cmdline config file
BOOT IMAGE=/boot/vmlinuz-4.4.0-59-generic
root=UUID=5f2e2232-4e47-4fe8-ae94-45ea749a5c92
```

- `BOOT IMAGE` : 메모리에 loadgkf 커널 이미지 파일
- `root` : root file system의 식별자

#### Linux의 boot loader

- Linux 커널 이미지는 압축파일
    - 메모리에 load된 후 압축 해제
- `initramfs` : boot loader가 부팅 중에 생성하는 임시 ram 파일 시스템
    - 실제 root file system을 지원하기 위해 설치해야하는 드라이버, 커널 모듈 등 포함
    - 커널이 시작되고 드라이버가 설치되면 커널은 이 파일 시스템을 적절한 root file system으로 교체


