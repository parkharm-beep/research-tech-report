---
tags: [technology, tech-note, database]
created: 2026-04-22
status: draft
---

# 그래프DB (Graph Database)

## 1. 개요 (Overview)
- **기술명:** 그래프DB (Graph Database)
- **정의:** 데이터를 노드(Node), 엣지(Edge), 속성(Property)으로 구성된 그래프 구조로 저장하고 관리하는 NoSQL 데이터베이스의 일종입니다.
- **배경:** 관계형 데이터베이스(RDBMS)가 복잡하게 얽힌 다대다(N:M) 관계를 처리할 때 발생하는 성능 저하(과도한 JOIN 연산)를 해결하고, 데이터 간의 연결성을 직관적으로 표현하기 위해 등장했습니다.

## 2. 주요 특징 (Key Features)
- **관계 중심 저장:** 데이터 간의 관계를 일급 객체(First-class Citizen)로 취급하여 직접 연결된 형태로 저장합니다.
- **인덱스 없는 인접성 (Index-free Adjacency):** 특정 노드에서 연결된 다른 노드를 찾을 때 인덱스 조회 없이 포인터를 따라 즉시 이동하므로 탐색 속도가 매우 빠릅니다.
- **유연한 스키마:** 고정된 테이블 구조 없이 필요에 따라 새로운 노드 타입이나 관계를 자유롭게 추가할 수 있습니다.
- **직관적 모델링:** 화이트보드에 그리는 모델 그대로 데이터베이스에 구현할 수 있어 비즈니스 로직과의 간극이 적습니다.

## 3. 작동 원리 (How it Works)
- **데이터 모델:** 주로 속성 그래프 모델(Property Graph Model)을 사용합니다.
    - **노드 (Nodes):** 개체(Entity)를 나타냅니다. (예: 사용자, 상품)
    - **엣지 (Edges):** 개체 간의 관계를 나타내며 방향성을 가질 수 있습니다. (예: 구매함, 친구임)
    - **속성 (Properties):** 노드와 엣지에 부여된 상세 정보입니다. (예: 이름, 가격, 날짜)
- **쿼리 언어:** SQL 대신 그래프 탐색에 최적화된 Cypher(Neo4j), Gremlin(Apache TinkerPop) 등의 언어를 사용하여 패턴 매칭 방식으로 데이터를 조회합니다.

## 4. 주요 활용 사례 (Use Cases)
- **소셜 네트워크 서비스:** 친구의 친구 찾기, 인맥 지도 분석 등 복잡한 관계 추적.
- **추천 엔진:** 사용자-상품-선호도 간의 관계를 분석하여 실시간 개인화 추천 제공.
- **사기 탐지 (Fraud Detection):** 비정상적인 거래 패턴이나 자금 세탁 경로를 네트워크 분석으로 식별.
- **IT 인프라/네트워크 관리:** 서버, 장비, 소프트웨어 간의 의존성 관리 및 영향도 분석.

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 복잡한 관계 데이터의 조회 및 탐색 성능이 RDBMS 대비 압도적으로 빠릅니다.
- 데이터 모델의 변경과 확장이 매우 유연합니다.
- 복잡한 비즈니스 관계를 시각적으로 이해하고 쿼리하기 쉽습니다.
### 단점 (Cons)
- 대규모 분산 환경에서의 수평적 확장(Sharding)이 RDBMS나 Document DB보다 까다로울 수 있습니다.
- 집계 연산(Aggregation)이나 전체 데이터 스캔 작업에는 비효율적일 수 있습니다.
- 전용 쿼리 언어(Cypher 등)를 학습해야 하는 비용이 발생합니다.

## 6. 관련 기술 (Related Technologies)
- [[지식그래프]]
- [[Neo4j DB]]
- Amazon Neptune
- Cypher
- Gremlin

## 7. 참고 자료 (References)
- [Neo4j: What is a Graph Database?](https://neo4j.com/developer/graph-database/)
