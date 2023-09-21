# 5. Deadlock Prevention

1. Mutual Exclusion
2. Hold and Wait
3. No Preemption
4. Circular Wait

---

Deadlock의 4가지 발생 조건 (Mutual Exclusion, Hold and Wait, No Preemption, Circular Wait) 중 하나라도 만족하지 않으면 deadlock이 발생하지 않음

## 1. Mutual Exclusion (상호 배제)

- Sharable resource에 상호 배제 접근을 요구하지 않게 함, dealock 발생하지 않음 e.g. read-only file
- mutex lock은 여러 thread가 동시에 접근할 수 없어야 함. 공유 불가능, deadlock 발생 가능

## 2. Hold and Wait

- 방법 1 : thread 시작 시 필요한 모든 resource를 할당
- 방법 2 : thread가 resource를 요청할 때는 다른 resource를 가지고 있지 않아야 함
    - thread가 가진 모든 resource를 release하고, 다른 resource를 요청해야 함
- 단점
    - resource 사용률이 낮음
    - 기아 발생 가능성

## 3. No Preemption

- 방법 1 :  resource를 보유한채 다른 wait이 필요한 resource를 요청한다면,
    - 보유한 resource를 선점되고, 다른 resource를 요청함
    - 즉, 기존의 보유한 resource를 release
- 방법 2 : resource를 요청할 때, resource 할당 가능 여부를 먼저 확인
    - 할당 가능 -> 할당
    - 할당 불가능 -> 해당 resource의 주인 thread가 _waiting_ 상태인지 확인
        - _waiting_ 상태라면, 해당 thread가 보유한 모든 resource를 preempt
        - _waiting_ 아니면, 요청한 thread가 _waiting_
- resource의 상태 변경이 쉬운 곳에 적합 e.g. CPU register, Database transaction
- mutex lock, semaphore 에는 부적합

## 4. Circular Wait (환형 대기)

- 현실적으로 dead lock을 방지할 수 있는 방법
- 모든 resource type에 순서를 부여하고, 각 thread는 오름차순으로 resource를 요청해야 함

### 예시

- resource type R = {R1, R2, ..., Rm} 의 각 R에 고유 번호를 부여
    - _F_ : _R_ -> _N_ (_N_ : 자연수)
    - e.g. Pthread program
        - `F(first_mutex) = 1`, `F(second_mutex) = 5`
- thread가 _Ri_ 를 요청했다면, 그 다음에는 _Rj_ (j > i)를 요청해야 함
    - 요청할 리소스보다 더 작은 번호의 리소스를 가지고 있다면, release 후 요청
    - e.g. - `F(first_mutex)` -> `F(second_mutex)`

### 증명 (모순 증명)

- 모든 resource type에 고유 번호가 있고,
- thread set _T_ = {_T1_, _T2_, ..., _Tn_} 이 circular wait 상태라고 가정
    - _Ti_ 은 _Ri_를 _waiting_
    - _Ti+1_ 이 _Ri_를 _holding_, _Ri+1_를 _waiting_
    - _F(R0)_ < _F(R1)_ < ... < _F(Rn)_ < _F(R0)_ (circular)
    - **_F(R0)_ < _F(R0)_ 이라는 모순된 결과**

### 주의점

- 환형 대기를 방지하는 것은 개발자에게 달렸음
    - lock의 순서를 결정하는 것은 큰 시스템에서 어려움
    - 대안으로 Java에서는 `System.identityHashCode()`를 사용해 lock의 순서를 결정
- lock을 동적으로 획득 가능한 경우, dead lock 방지 못함

````
void transaction(Account from, Account to, double amount){
    
    mutex lock1, lock2;
    lock1 = get_lock(from); // mutex lock을 동적으로 획득
    lock2 = get_lock(to); // mutex lock을 동적으로 획득 
    
    acquire(lock1);
        acquire(lock2);
        
            withdraw(from, amount);
            deposit(to, amount);  
            
        release(lock2);
    release(lock1);
    
}

....

// thread 1
transaction(checking_account, savings_account, 100.0);

// thread 2
transaction(savings_account, checking_account, 200.0); // deadlock 발생
````
