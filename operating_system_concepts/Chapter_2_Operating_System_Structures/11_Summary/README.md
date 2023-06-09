# 11. Summary

- OS는 사용자와 프로그램에게 서비스를 제공함으로써 프로그램이 실행할 수 있는 환경을 제공한다.
- OS와 상호작용하는 방법
    - command interpreter
    - graphical user interface
    - touch screen interface
- System call은 OS 서비스에 대한 인터페이스를 제공한다. 프로그래머는 System call API를 사용하여 System call 한다.
- System call의 주요 기능
    - 프로세스 제어
    - 파일 관리
    - 장치 관리
    - 정보 유지
    - 통신
    - 보호
- UNIX, Linux의 System calll API는 표준 C 라이브러리
- OS는 사용자에게 유틸리티를 제공하는 시스템 프로그램 모음이 있다.
- linker는 여러 object 모듈을 단일 이진 실행파일로 결합하고, loader는 실행파일을 메모리에 적재한다.
- 어플리케이션이 OS에 종속되는 이유
    - 이진파일 포맷
    - CPU 마다 다른 명령어 집합
    - System call
- OS는 특정 목적에 특화되어 설계되어야 하기 떄문에, 특정 메커니즘을 기반으로 정책을 설정하여 설계한다.
- monolithic OS는 구조가 없다.
    - 모든 기능이 단일 정적 이진파일에 위치하여 하나의 주소 공간에서 실행됨
    - 수정은 어렵지만, 효율성이 좋음
- Layered OS는 몇개의 분리된 계층으로 나뉘어지는 구조
    - 최하부는 하드웨어 인터페이스, 최상단은 사용자 인터페이스
    - 성능 문제로 인해 이상적인 구조는 아님
- microkernel 방식:  작은 kernel을 가지고, 대부분의 서비스는 유저 레벨 어플리케이션으로 실행
- modular 방식 : 모듈을 통해 OS 서비스 제공, 모듈은 런타임에 load, remove 가능
    - 대부분의 현대 OS 들이 하이브리드 채택 <sub>modular + microkernel</sub>
- boot loader는 OS를 메모리에 적재, 초기화, 시스템 실행을 시작한다
- OS의 성능은 counter와 tracing으로 모니터링 할 수 있다.
    - counter : 프로세스 별, 시스템 전반적인 통계 정보
    - tracing : 특정 프로세스의 특정 이벤트를 추적