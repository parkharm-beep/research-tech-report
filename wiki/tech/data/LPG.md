---
tags: [technology, tech-note, data]
created: 2026-05-12
source_count: 0
status: stub
---

# LPG (Labeled Property Graph)

## 1. 개요 (Overview)
- **기술명:** LPG (Labeled Property Graph, 레이블·속성 그래프)
- **정의:** 노드(Node)와 엣지(Edge) 각각에 레이블(유형)과 속성(키-값 쌍)을 부여할 수 있는 그래프 데이터 모델로, [[Neo4j DB]] 등 주요 그래프 데이터베이스의 표준 모델이다
- **배경:** [[RDF]]의 트리플 기반 모델이 시맨틱 웹 중심이라면, LPG는 애플리케이션 개발에 더 직관적이고 유연한 그래프 표현을 제공하여 관계형 DB를 대체하는 그래프 DB의 주류 모델이 됐다.

## 2. 주요 특징 (Key Features)
- **레이블 (Label):** 노드·엣지의 유형 분류 (예: `:Person`, `:KNOWS`)
- **속성 (Property):** 노드·엣지에 키-값 쌍으로 추가 데이터 저장 (예: `name: "Alice"`)
- **방향성 엣지:** 관계는 방향을 가짐 (예: Alice -[:FOLLOWS]→ Bob)
- **Cypher 쿼리:** [[Neo4j DB]] 등에서 사용하는 선언적 그래프 쿼리 언어

## 3. 작동 원리 (How it Works)
- 데이터를 노드(엔티티)와 엣지(관계)로 모델링
- 각 노드에 여러 레이블 부여 가능 (예: `:Person:Employee`)
- 그래프 탐색: 패턴 매칭으로 관계를 따라 데이터 탐색 (JOIN 없이 관계 순회)

```cypher
MATCH (p:Person)-[:KNOWS]->(friend:Person)
WHERE p.name = 'Alice'
RETURN friend.name
```

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 소셜 네트워크 — 사용자 간 팔로우·친구 관계 저장·탐색
- 사례 2: 지식 그래프 — 개념·엔티티 간 관계 표현 ([[지식그래프]])
- 사례 3: 사기 탐지 — 계좌·거래·사용자 간 이상 패턴 탐지

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 다중 홉(Multi-hop) 관계 탐색이 관계형 DB의 재귀 JOIN보다 훨씬 빠름
- 스키마 유연성 — 노드·엣지 종류를 자유롭게 추가
### 단점 (Cons)
- SQL 생태계에 비해 개발자 풀이 적음
- 복잡한 집계 연산은 관계형 DB가 유리

## 6. 관련 기술 (Related Technologies)
- [[Neo4j DB]]
- [[그래프DB]]
- [[지식그래프]]
- [[RDF]]
- [[그래프RAG]]

## 7. 참고 자료 (References)
- Neo4j 공식 문서 (neo4j.com/docs)
