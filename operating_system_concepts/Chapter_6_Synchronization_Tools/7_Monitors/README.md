# 7. Monitors

1. Monitor Usage
2. Implementing a Monitor Using Semaphores
3. Resuming Processes within a Monitor

---

#### mutex lock과 semaphore의 오류 가능성

- `signal()`이 `wait()` 보다 먼저 실행되면, 동시에 2개이상의 프로세스가 임계영역 진행 가능
- 프로그램이 `signal()`을 `wait()` 으로 대체했을 때, 프로세스는 영구적으로 block
- `wait()`이나 `signal()` 중 하나라도 누락되면 상호 배제가 무시되고, 프로세스가 영구적으로 block
 
## 1. Monitor Usage

````
monitor monitor1{
    /* shared variable declarations */
    
    funtion P1(...){
           ...
    }
    
    funtion P2(...){
           ...
    }
    
    ...
    
    initialization code(...){
           ...
    }
}

````

- Abstract Data Type (ADT) : 프로그래머가 정의한 자료형
- ADT를 사용해서 함수와 데이터를 캡슐화
- `monitor type` :  프로그래머가 정의한 monitor와 함께 구현된 상호배제 명령 ADT
- 프로세스들이 `monitor type`을 직접 호출할 수 없음
    - 내부 지역변수, 함수에 접근 불가

<img src="img.png"  width="50%"/>

- 동시에 하나의 프로세스만이 `monitor`와 함께 활동할 수 있음

<img src="img_1.png"  width="50%"/>

- 프로그래머는 `contition` 구조를 추가로 정의해서 동기화 구현
    - `condition x, y;`
    - `x.wait()` : 다른 프로세스가 `x.signal()`을 호출할 때까지 프로세스를 block
    - `x.signal()` : `x.wait()`을 호출한 프로세스 중 하나를 재개

#### 상세 동작

- P : `x.signal()`을 호출한 프로세스
- Q : `x.wait()`을 호출한 프로세스
- 두가지 시나리오 가능
    - Signal and wait : P는 Q가 모니터를 떠날떄까지 기다리거나, 다른 condition을 기다림
    - Signal and continue : Q는 P가 모니터를 떠날때까지 기다거나, 다른 condition을 기다림

## 2. Implementing a Monitor Using Semaphores

## 3. Resuming Processes within a Monitor
