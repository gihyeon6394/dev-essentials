# 10. Indexing of Spatial and Temporal Data

1. Indexing of Spatial Data
2. Indexing Temporal Data

---

- B+-Tree index, hash index는 1차원 데이터에 적합
- Spatial data (2차원 이상), Temporal Data (시계열)는 다른 방식의 index 필요
    - tupl이 시간 간격이랑 연관된 경우 등

## 1. Indexing of Spatial Data (2차원 이상)

### Spatial Data?

- Spatial Data : 2차원 이상 공간에 대한 참조를 하는 데이터
- e.g. 레스토랑의 위/경도 쌍, 한 나라의 공간 범위 (위/경도 쌍으로 이루어진 다각형)

### B+-Tree index의 한계

- query 성능 낮음
- B+-Tree index : (위, 경도) 쌍의 복합 인덱스 생성
- **range query** 에 비효율
    - e.g. 사용자 위, 경도의 500 미터 이내의 모든 레스토랑?
- **rectangular range query** : 위, 경도의 사각형 범위 내의 모든 레스토랑?
    - e.g. 한 나라의 공간 범위 내의 모든 레스토랑?
- **nearest neighbor** 에 비효율
    - e.g. 사용자 위, 경도에서 가장 가까운 레스토링?

### 1차원 데이터를 이진트리 (B+-Tree)로 표현

- 연속적으로 공간을 나누는 방식으로 동작
    1. 나눔
    2. 왼쪽은 왼쪽 서브트리, 오른쪽은 오른쪽 서브트리 접근
    3. 반복

### k-d tree

<img src="img.png"  width="60%"/>

- 초기에 다차원 데이터를 indexing 하는 구조
- partitioning : 각 level이 공간을 둘로 나눔
    - level 이 높아지면서 공간을 둘로 나눔
- partitioning 1번마다 대략 반의 데이터 (point) 가 나뉘어짐
    - 각 node에 저장된 point 수가 최대 point 수보다 적어지면 partitioning 중단
- 위 표에서
    - leaf node 별 최대 point 수 : 1
    - 숫자는 k-d tree의 level을 나타냄
- Rectangular range queries, Nearest neighbor queries 에 효율적

#### 동작 : Rectangular range queries

- _x_ 차원이 50 ~ 80 and _y_ 차원이 40 ~ 70인 모든 point?
- 다음 탐색을 반복해서 수행

1. root에서 시작
2. _x_ 차원에서 분할되었고, point _x<sub>i</sub>_ 에서 paritioning 되었다고 가정
    - 왼쪽 서브트리 : _x<sub>i</sub>_ 보다 작은 _x_ 차원의 point
    - 오른쪽 서브트리 : _x<sub>i</sub>_ 보다 같거나 큰 _x_ 차원의 point
    - _x_ 가 50 ~ 80 에 속하는 범위를 가진 서브트리로 재귀 탐색
3. leaf node에 도달하면 검색 종료

#### k-d-B tree

- k-d tree 확장
- 각 내부 노드가 2개 이상의 child node를 가지도록 함
    - B-tree 가 binary tree 확장한 것처럼
    - tree의 height를 줄이기 위함
- 2차 저장소에 저장 시 k-d tree 보다 효율적
- 다른 대체 : quadtree
    - 각 node에서 2차원 공간을 4개로 나눔

### R-tree : 다각형 범위를 가진 데이터를 indexing 하는 구조

<img src="img_1.png"  width="70%"/>

- k-d tree는 2차원까지만 효율적
- R-tree : B+-tree 같이 leaf node에 index된 객체를 저장하는 균형 트리
- **bounding box** : 각 노드는 범위와 연결되지 않고, bounding box로 연결됨
- leaf node : leaf node에 속하는 모든 객체를 포함하는 bounding box
    - index된 객체 저장
- 내부 노드 : 자식 노드의 bounding box를 포함하는 bounding box
    - 자식 노드의 포인터 저장
- 실선 : 사각형 집합 element
- 점선 : bounding box

## 2. Indexing Temporal Data (시계열)

### Temporal Data?

| course_id | course_title           | time      |
|-----------|------------------------|-----------|
| 101       | 데이터베이스 기초              | 2020~2021 |
| 101       | 데이터베이스 기초(2023 개정)     | 2022~2023 |
| 101       | 데이터베이스 기초(2024 개정)     | 2024~9999 |
| 101       | 데이터베이스 임시 강의 (2025 여름) | 2025      |
| 102       | 데이터베이스 응용              | 2020~9999 |

- 시간 기간을 가지는 데이터
- 시간 기간은 각 기간에 해당하는 tuple과 연관됨
    - 같은 _course_id_ 에도 _time_ 에 따라 다른 _course_title_ 을 가질 수 있음
- **time interval** : 시작 시각, 종료 시각
    - 시작시각만 있거나 종료시각만 있을 수도 있음
    - 무한대 시각을 위해 9999-12-31 같은 시각을 사용하기도 함
- 일반적으로 유효한 time interval은 하나의 tuple로 특정할 수 없음
    - e.g. 학생은 2020-01-01 ~ 2020-06-15 까지 데이터베이스 기초를 수강하고,
        - 2020-12-01 ~ 2021-06-15 까지 데이터베이스 응용을 수강할 수 있음

### _course_id_ 에 index가 있을 때

- _coourse_id_ 가 _101_ 이고, _time_ 이 _2020-01-05_ 인 tuple을 찾을 때,
    - _course_id_ 가 _101_ 인 모든 tuple을 찾음
    - 성능 저하 _couse_id_ 가 _101_ 인 tuple이 많다면?

### 대안 1 : R-tree

- index 된 tuple을 2차원으로 봄 (_course_id_, _time_)

#### 이슈 : 시작 시각이 무한대일 수 있음 (9999와 같은 값)

- R-Tree는 bounding box를 사용하므로, 성능이 떨어질 수 있음
- 종료시각이 무한대인 tuple을 별도의 index에 저장
    - 무한대 tuple :  B+-Tree (_course_id, _time (시작시각)_) 으로 구성
    - 무한대 아닌 tuple : R-Tree 구성
- 특정 시점 _t<sub>i</sub>_ 에 대해 value _v_ 로 조회 시
    - 양쪽 index 모두 조회

### 대안 2 : interval B+-Tree

- 1차원이지만 시간 간격을 표현하기에 특화
- R-Tree 보다 나은 복잡도 제공
- 대부분의 DB는 R-Tree를 간단히 구현해서 제공
