---
tags: [technology, tech-note, data]
created: 2026-05-12
source_count: 0
status: stub
---

# Elasticsearch

## 1. 개요 (Overview)
- **기술명:** Elasticsearch
- **정의:** [[Lucene]] 기반의 분산형 오픈소스 검색·분석 엔진으로, RESTful API로 대용량 데이터의 전문(full-text) 검색, 로그 분석, 실시간 집계를 수행하는 플랫폼
- **배경:** 2010년 Shay Banon이 개발한 Compass 프로젝트에서 출발하여 Elastic이 상용화했다. Logstash·Kibana와 함께 ELK 스택을 구성하며, 최근 [[Sparse 임베딩]] 기반 뉴럴 검색(ELSER)도 지원한다.

## 2. 주요 특징 (Key Features)
- **분산 아키텍처:** 샤드·레플리카 기반으로 수평 확장 및 고가용성 보장
- **역색인(Inverted Index):** 단어→문서 매핑으로 빠른 전문 검색 (내부적으로 [[Lucene]] 사용)
- **RESTful API:** JSON 기반 쿼리로 직관적인 CRUD 및 집계 쿼리
- **뉴럴 검색 지원:** ELSER(Elastic Learned Sparse EncodeR)로 [[Sparse 임베딩]] 기반 시맨틱 검색
- **kNN 벡터 검색:** Dense 벡터 필드로 [[Vector Search]] 지원 (8.x~)

## 3. 작동 원리 (How it Works)
- 문서를 JSON 형태로 인덱싱 → Lucene이 역색인 생성
- 쿼리 수신 시 전체 샤드에 병렬 검색 후 결과 통합
- BM25 스코어링 기본, 커스텀 스코어링 가능

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 로그 분석 (ELK 스택) — Logstash 수집 → Elasticsearch 저장·분석 → Kibana 시각화
- 사례 2: 전자상거래 상품 검색 — 키워드·필터·랭킹 통합
- 사례 3: RAG 파이프라인 — [[BM25]] + 벡터 검색 하이브리드

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 대규모 데이터에서 빠른 전문 검색
- 키워드·벡터·집계를 단일 엔진으로 처리
- 활발한 생태계 (Kibana, APM, SIEM 등)
### 단점 (Cons)
- 운영 복잡도 높음 (샤드 튜닝, 힙 메모리 관리)
- 실시간 업데이트 성능이 전통 DB 대비 낮음
- 라이선스 정책 변경 (2021년 Apache → SSPL)

## 6. 관련 기술 (Related Technologies)
- [[Lucene]]
- [[BM25]]
- [[Sparse 임베딩]]
- [[Vector Search]]
- [[RAG]]

## 7. 참고 자료 (References)
- Elastic 공식 문서 (elastic.co/docs)
