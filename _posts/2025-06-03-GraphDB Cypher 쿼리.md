---
layout: post
title: GraphDB Cypher 쿼리
categories: [GraphDB]
comments: true
---

# 그래프 데이터베이스(Graph DB) Cypher 쿼리

## 1. Cypher 쿼리 예시

Cypher는 Neo4j에서 사용하는 **선언형 쿼리 언어**로, 그래프 구조를 텍스트로 직관적으로 표현할 수 있도록 설계되었습니다.

아래는 Cypher 쿼리의 기본적인 사용 예시입니다.

---

### 🔹 노드 생성

```cypher
CREATE (a:Person {name: 'Alice'})
CREATE (b:Person {name: 'Bob'})
```

- `Person`이라는 레이블을 가진 두 개의 노드를 생성
- 각각의 노드에는 `name` 속성이 부여됨

---

### 🔹 관계 생성

```cypher
CREATE (a)-[:FRIENDS_WITH]->(b)
```

- `a` 노드와 `b` 노드를 `FRIENDS_WITH`라는 관계로 연결
- 관계는 방향성을 가짐 (단방향)

> 관계에도 속성을 부여할 수 있습니다:
> ```cypher
> CREATE (a)-[:FRIENDS_WITH {since: 2020}]->(b)
> ```

---

### 🔹 관계 탐색

```cypher
MATCH (p:Person {name: 'Alice'})-[:FRIENDS_WITH]->(friend)
RETURN friend.name
```

- 이름이 "Alice"인 노드를 찾고, 해당 노드로부터 이어진 `FRIENDS_WITH` 관계를 따라 연결된 `friend` 노드를 탐색
- 친구의 이름을 반환

---

### 🔹 노드/관계 업데이트

```cypher
MATCH (p:Person {name: 'Alice'})
SET p.age = 30
```

- "Alice" 노드에 `age` 속성을 추가 또는 업데이트

---

### 🔹 노드 삭제

```cypher
MATCH (p:Person {name: 'Alice'})
DETACH DELETE p
```

- 노드와 그에 연결된 모든 관계를 함께 삭제 (`DETACH DELETE` 사용)

---

### 🔹 전체 그래프 조회 (예시용)

```cypher
MATCH (n)-[r]->(m)
RETURN n, r, m
```

- 모든 노드와 관계를 조회하여 시각화할 때 사용

---

Cypher는 관계 중심의 질의를 매우 직관적으로 표현할 수 있는 강력한 언어입니다.  
SQL과 달리 관계를 표현할 때 JOIN이 필요 없으며, 연결 구조를 자연스럽게 탐색하는 데 최적화되어 있습니다.

> 💡 Cypher에서 `()-[]->()` 구조는 항상  
> `(출발 노드)-[관계]->(도착 노드)`를 의미합니다.

---

## 2. Neo4j 실습 환경 소개

### 2.1 Neo4j Desktop
- 로컬 PC에 설치 가능한 GUI 기반 개발 툴
- 그래프 생성, 시각화, 쿼리 실행 등 통합 환경 제공
- 개발 및 테스트용으로 적합

### 2.2 Neo4j Aura
- Neo4j에서 제공하는 공식 클라우드 서비스
- 별도 설치 없이 웹에서 데이터베이스 생성 및 운영 가능
- 무료 플랜 제공 (Aura Free)

### 2.3 Neo4j Browser
- 웹 기반 UI (`http://localhost:7474`)
- Cypher 쿼리 실행 및 그래프 시각화 기능 탑재
- 빠른 구조 확인과 쿼리 실험에 유용

### 2.4 Neo4j 드라이버 (Python 예시)

```python
from neo4j import GraphDatabase

# Neo4j에 연결
uri = "bolt://localhost:7687"
driver = GraphDatabase.driver(uri, auth=("neo4j", "password"))

# 트랜잭션으로 쿼리 실행
def get_friends(tx, name):
    query = "MATCH (p:Person {name: $name})-[:FRIENDS_WITH]->(friend) RETURN friend.name"
    result = tx.run(query, name=name)
    return [record["friend.name"] for record in result]

with driver.session() as session:
    friends = session.read_transaction(get_friends, "Alice")
    print(friends)
```
