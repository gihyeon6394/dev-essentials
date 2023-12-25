# 6. Thrashing

1. Cause of Thrashing
2. Working-Set Model
3. Page-Fault Frequency
4. Current Practice

---

- 프로세스가 충분한 frame을 갖지 못할 때, page-fault가 발생해도 여유 page가 없다면?
- **thrashing** : 프로세스가 실제로 일을 하지 않고, page-fault가 반복해서 일어남
    - 문제점 : 실제 실행시간보다 페이징하는데 더 많은 시간이 소요됨 -> 성능 문제

## 1. Cause of Thrashing

### 시나리오 global page-replacement algorithm : 멀티 프로그래밍이 높아져서 page fault가 늘어남

![img.png](img.png)

1. OS가 CPU 활용률을 모니터링 중 -> CPU 사용률이 너무 낮으면 멀티프로그래밍 높임
2. 더많은 프로세스로부터 page fault 발생, 필요한 프레임들이 많아짐
3. paging device에 page 요청을 queue로 관리
4. paging device queue가 빔
3. 실제 CPU 사용률이 낮아짐 -> **CPU 스케쥴러가 멀티 프로그래밍을 높임**
4. 악순환 : CPU 사용률이 낮아지고, page fault가 더 많이 발생하고 CPU 스케쥴러는 멀티 프로그래밍을 높임

### 해결책 : Local replacement algorithm or Priority replacement algorithm

- Local replacement algorithm은 자신의 frame 에서만 page replacement를 수행
    - 하나의 프로세스에서 Thrashing이 발생해도 다른 프로세스에 영향을 주지 않음
- **문제점** : 전체 service time은 여전히 낮음

#### 해결책 : locality model

![img_1.png](img_1.png)

- Local replacement algorithm에서 프로세스가 필요한 만큼의 frame을 할당시킴
- 프로세스가 실제로 얼만타 많은 frame을 사용하는지 모니터링
- locality : **공통적으로** 활발하게 사용되는 page 집합
    - 프로세스는 보통 여러 locality를 가지고, locality는 overlap될 수 있음
        - overlap : 글로벌 변수 등
        - 위 그림에서 page number 18, 19, 20은 _a_ 와 _b_ 의 locality에 모두 속함

## 2. Working-Set Model

## 3. Page-Fault Frequency

## 4. Current Practice