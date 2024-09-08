# Part five. Mass Storage Structure

- Computer System은 데이터, 파일을 영구적으로 저장하기위해 대용량 저장장치를 사용한다.
- 현대 컴퓨터는 hard disk, 비휘발성 메모리 장치를 사용해 2차 저장소 구현하여 대용량 저장장치로 사용
- 2차 저장소의 기능
    - 하나 이상의 문자를 전송
    - 순차적, 랜덤하게 접근 가능
    - 데이터를 동기/비동기적으로 전송
    - 전용으로 사용되거나 공유 가능
    - read-only, read-write 가능
    - 대부분의 2차 저장소는 컴퓨터서 가장 느린 장치
- OS는 애플리케이션이 이런 다양한 2차 저장소의 기능을 사용할 수 있도록 해야함
- OS I/O subsystem
    - 시스템의 다른 부분들에게 간단한 인터페이스 제공
    - I/O에 대한 최대의 동시성을 제공 (병목 최소화)

---

# 11. Mass Storage Structure

- mass storage의 구조
- 모던 컴퓨터의 대용량 스토리지 시스템은 2차 저장장치 (secondary storage)로 구현
    - HDD (hard disk drive), NVM (nonvolatile memory) 디바이스를 통해 제공
    - 3차 저장장치 : 마그네틱 테이프, optical disk, cloud storage 등도 있음
- 대부분의 시스템이 HDD, NVM을 사용
- HDD, VNM의
    - 물리적 구조
    - 스케줄링 알고리즘
    - 디바이스 포맷팅, boot block, damaged block, swap space 관리
- RAID 시스팀
- _non-volatile storage_ (NVS) : mass storage 를 칭하는 종합적인 용어
    - drive라고도 함

> ### CHAPTER OBJECTIVES
>
> - 다양한 2차 저장장치의 물리적 구조
> - mass-storage 장치의 성능
> - I/O 스케쥴링 알고리즘 평가
> - OS가 제공하는 mass storage 서비스 (e.g. RAID)

1. Overview of Mass-Storage Structure
2. HDD Scheduling
3. NVM Scheduling
4. Error Detection and Correction
5. Storage Device Management
6. Swap-Space Management
7. Storage Attachment
8. RAID Structure
9. Summary
