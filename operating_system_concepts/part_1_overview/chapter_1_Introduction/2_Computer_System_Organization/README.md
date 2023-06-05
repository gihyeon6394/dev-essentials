# 2. Computer-System Organization

1. Interrupts
    1. Overview
    2. Implementation
2. Storage Structure
3. I/O Structure

---

<img src="img.png"  width="40%"/>

#### device controller <sup>장치 제어기</sup>

- 1 Computer, 여러 개의 장치 제어기 <sup>device controller</sup>
- Bus 를 통해 연결
- Bus : 공유 메모리와 컴포넌트를 연결
- 각 장치 제어기는 특정 유형의 디바이스 장치를 담당
    - ex. disk drive, audio controller, USB controller 등
- 장치 제어기에 따라 여러개의 컴포넌트 연결 가능
    - ex. USB port는 USB hub를 통해 여러 장치와 연결
- 장치 제어기는 일부 로컬 버퍼 저장소와 특수 목적의 레지스터 세트를 유지
- 장치 제어기 주변 장치와 로컬 버퍼 저장소 간의 데이터 이동 책임

#### device driver <sup>장치 드라이버</sup>

- 장치 제어기 마다 존재
- 장치 제어기를 이해하고, OS 에게 장치에 대한 적절한 인터페이스 제공
- CPU와 장치 제어기는 병렬로 실행 가능 <sub>메모리 사이클에서 경쟁</sub>
- 공유 메모리에 접근 순서를 올바르게 하기 위해, memory controller 는 메모리의 접근을 동기화

#### 시스템의 3가지 지점

- Interrupt
- storage structure
- I/O structure

---

## 2.1 Interrupts

#### 예시 : 키보드 타이핑

1. 키보드를 누름
2. 장치 드라이버는 device controller 에게 적절한 레지스터 로드
3. device controller 가 레지스터의 내용을 검사하여 수행할 작업 결정
    - ex. 키보드 장치를 통해 문자를 읽어라.
4. device controller가 문자를 device에서 로컬 버퍼로 전송
5. 전송이 끝나면 dvice driver에게 입력이 끝났음을 알림
6. 장치 드라이버는 제어를 다른 OS 일부에게 넘김

#### interrupt : 어떻게 device controller 가 장치 드라이버에게 제어가 종료되었음을 알릴 수 있음

### 2.1.1 Overview

### 2.1.2 Implementation

## 2.2 Storage Structure

## 2.3 I/O Structure
