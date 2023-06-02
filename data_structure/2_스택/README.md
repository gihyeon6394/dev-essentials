# 2. 스택

- 스택의 개념과 추상 자료형
- 스택의 응용과 구현
- 사칙 연산식의 전위 / 후위 / 중위 표현

## 스택의 개념과 추상 자료형

### 스택의 정의

<img src="img.png"  width="30%"/>

- 가장 먼저 입력된 자료가 가장 나중에 출력되는 관계
- 객체와 객체가 저장된 순서를 기억하는 자료구조
- 0 개 이상의 원소를 갖는 유한 순서 리스트
- push()<sup>add</sup>, pop()<sup>delete</sup> 연산이 한곳에서 발생

### 스택의 추상 자료형

#### CreateS 연산

- Stack CreateS(maxStackSize) ::= 스택의 크기가 maxStackSize인 빈 스택을 생성 후 반환

#### Push 연산

- Element Push(S, newItem) ::= if(isFull(S)) then 오류 출력 else S에 newItem을 삽입

#### Pop 연산

- Element Pop(S) ::= if(isEmpty(S)) then 오류 출력 else S의 top에 있는 원소를 삭제 후 반환

#### Pop & Push 연산 실행

```
createS(3);
push(S, 1);
push(S, 2);
pop(s);
push(S, 3);
```

## 스택의 응용과 구현

## 사칙 연산식의 전위 / 후위 / 중위 표현

