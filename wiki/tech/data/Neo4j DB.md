---
tags: [technology, tech-note, data]
created: 2026-05-12
source_count: 0
status: stub
---

# Neo4j DB

## 1. 개요 (Overview)
- **기술명:** Neo4j
- **정의:** [[LPG]](Labeled Property Graph) 모델을 채택한 세계 최대 점유율의 상용 그래프 데이터베이스로, Cypher 쿼리 언어로 복잡한 관계 데이터를 직관적으로 탐색한다
- **배경:** 2007년 스웨덴에서 설립된 Neo4j Inc.가 개발했다. 관계형 DB의 복잡한 JOIN을 대체하는 그래프 탐색으로 소셜 네트워크·추천·지식 그래프 분야에서 광범위하게 채택됐다.

## 2. 주요 특징 (Key Features)
- **[[LPG]] 기반:** 노드·엣지·레이블·속성의 유연한 그래프 모델
- **Cypher 쿼리:** `MATCH (n)-[r]->(m)` 형태의 선언적 패턴 매칭 쿼리 언어
- **네이티브 그래프 스토리지:** 관계를 포인터로 직접 저장 → 멀티홉 탐색 성능 우수
- **APOC 라이브러리:** 그래프 알고리즘(PageRank, 최단 경로, 커뮤니티 탐지) 플러그인
- **벡터 인덱스 지원:** LLM 임베딩과 결합한 [[그래프RAG]] 구현 가능

## 3. 작동 원리 (How it Works)
- 데이터를 노드와 관계(엣지)로 저장, 물리적으로 포인터 기반 연결
- Cypher 쿼리로 패턴 매칭 → 관계를 따라 그래프 순회
- 트랜잭션 지원(ACID) + 클러스터 모드(Enterprise)로 고가용성

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 추천 시스템 — 사용자-아이템 구매 관계 그래프에서 협업 필터링
- 사례 2: 지식 그래프 — 개념 간 관계를 탐색하는 [[그래프RAG]] 구축
- 사례 3: IT 인프라 의존성 맵 — 서비스·서버·API 간 의존 관계 시각화

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 깊은 관계 탐색(멀티홉)에서 관계형 DB 대비 압도적 성능
- 직관적인 Cypher 문법
### 단점 (Cons)
- 대규모 클러스터 운영은 Enterprise 라이선스 필요 (비용)
- 정형 집계·OLAP 워크로드는 관계형 DB가 유리

## 6. 관련 기술 (Related Technologies)
- [[LPG]]
- [[그래프DB]]
- [[지식그래프]]
- [[그래프RAG]]
- [[Vector Database]]

## 7. 참고 자료 (References)
- Neo4j 공식 문서 (neo4j.com/docs)
