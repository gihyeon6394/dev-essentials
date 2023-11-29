# 6. Graph DataBases

### 그래프 데이터베이스 예시

- 그래프 데이터 타입을 다루는 DB
- E-R 모델은 모두 그래프 데이터 타입으로 표현 가능

| 예시             | node         | edge       |
|----------------|--------------|------------|
| social network | person       | friendship |
| road network   | intersection | road       |
| web            | webpage      | hyperlink  |

### E-R Model과 그래프 데이터 베이스

| table | column                         |
|-------|--------------------------------|
| node  | ID, label, node_data           |
| edge  | fromID, toID, label, edge_data |

#### 한계

- 각 node의 attribute가 고정임

### 그래프 데이터베이스의 특징

- 각 node가 고유의 attribute set을 가짐
- 다양한 타입의 edge, edge 별 고유의 attribute set

### 예시 : Neo4j

- node와 edge를 활용해 relation 정의
- path query를 위한 query language 제공 (SQL로는 어려움)
- SQL보다 성능 최적화
- graph 시각화 기능 제공
- parallel processing 미지원 (2018)

#### 쿼리 예시 1

- node : _instructor_, _student_
- edge : _advisor_

```sql
match (i:instructor)<-[:advisor]-(s:student)
where i.deptname = 'Comp. Sci.'
return i.ID as ID, i.name as name, collect(s.name) as advisees
```

- `match` 절 : node와 edge를 활용해 relation 정의
    - SQL의 `join`과 유사

#### 쿼리 예시 2 : recursive traversal of edges

- _course_ 에 대한 직/간접적 필요조건 코스 파악을 위한 쿼리
- _prereq(course_id, prereq_id)_ : _course_id_가 _prereq_id_의 선수과목인지 여부를 나타내는 edge

```sql
match (c1:course)-[prereq*1..]->(c2:course)
return c1.course_id, c2.course_id
```

- `*1..` : 1개 이상의 edge를 통해 연결된 모든 node를 찾음

### 예시 : PageRank

- hyperlink를 node로 간주, hyperlink간의 이동을 edge로 간주
- SNS에서 유용

### parallel graph processing

#### 방법 1. Map-reduce and algebraic framework

- Graph를 relation으로 표현, 각 step을 join으로 표현
- Graph를 parallel storage system에 저장
- Map-reduce를 통해 parallel processing (e.g. Spark)
- 반복 연산에는 적합하지 않음

#### 방법 2. Bulk synchronous parallel (BSP) model

- Graph를 memory에 저장
- node를 각 머신에 partitioning
- BSP framework가 각 노드에게 적용가능한 method 제공
- 각 node는 state (data)를 유지
- **superstep** : 각 노드가 method를 수행하는 단계
    - 이웃 node와 message 송/수신
    - message에 대해 node에 적절한 처리 (state 업데이트 등)
- e.g. _Pregel_ (Google), _Giraph_ (Apache)

### 예시 : GraphX (Apache Spark)

- Apache Spark의 그래프 처리 컴포넌트
- Pregel 기반의 API 제공
- map function : 각 node, edge에 적용
- aggeregation function : 사용자가 정의한 function
    - messge 생성, 이웃 node 전송
    - message aggregation