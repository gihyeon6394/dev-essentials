# 3. System Calls

1. Example
2. Application Programming Interface
3. Types of System Calls

---

- System Call은 OS에서 제공하는 서비스에 대한 인터페이스를 제공
- C, C++ 로된 함수 형태로 제공
- 하드웨어에 직접 접근 작업의 경우 어셈블리어 명령어 작성 필요

## 1. Example

#### 파일 복사

```shell
cp in.txt out.txt
```
<img src="img.png"  width="50%"/>

1. 프로그램 입력에 input 파일, output 파일 지정
    - UNIX : 파일명, 와일드 카드 등의 형태로 지정 가능
    - 대화형 OS :  프로그램이 사용자에게 파일명 물어봄
    - 마우스/키보드 기반 OS : 파일 목록을 디스플레이로 표시, 사용자가 입력장치로 선택, 많은 I/O 발생
2. 파일 세팅 :  프로그램이 input 파일을 열고 <sup>1</sup>, output 파일을 만들고 엶 <sup>2</sup>
    - 각자 system call 발생하고, 각자 에러 핸들링 필요
        - input 파일 : 파일이 없거나, 사용자가 읽기 권한이 없거나 등, 비정상 종료 시킴
        - output 파일 : 파일이 이미 있으면 삭제 후 생성 등
3. 파일 복사 : input을 읽고 <sup>system call</sup> output에 쓰기<sup>system call</sup> 반복
    - 각 작업에서 상태 정보를 반환해야함
    - 읽기 : 파일의 끝에 도달했는지, 파일의 끝에 도달했다면 EOF 반환 등
    - 쓰기 : 디스크가 가득 찼는지, 가득 찼다면 에러 반환 등
4. 복사 완료 : 두 파일을 닫고,메시지를 콘솔에 출력

## 2. Application Programming Interface




## 3. Types of System Calls
