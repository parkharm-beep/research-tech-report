---
tags: [technology, tech-note, data]
created: 2026-05-12
source_count: 0
status: stub
---

# Apache Lucene

## 1. 개요 (Overview)
- **기술명:** Apache Lucene
- **정의:** Java로 작성된 고성능 오픈소스 전문(full-text) 검색 라이브러리로, [[Elasticsearch]]·Solr 등 주요 검색 엔진의 핵심 엔진으로 사용된다
- **배경:** 1999년 Doug Cutting이 개발하여 2001년 Apache Software Foundation에 기증했다. 역색인(Inverted Index) 구현의 사실상 표준으로, 대부분의 엔터프라이즈 검색 솔루션의 기반이다.

## 2. 주요 특징 (Key Features)
- **역색인 (Inverted Index):** 단어→문서 위치 매핑으로 O(1) 수준의 빠른 키워드 검색
- **세그먼트 기반 저장:** 쓰기 효율을 위해 불변(immutable) 세그먼트 단위로 인덱스 관리
- **풍부한 쿼리 타입:** BooleanQuery, PhraseQuery, FuzzyQuery, RangeQuery 등
- **[[BM25]] 스코어링:** 기본 관련성 스코어링 알고리즘
- **벡터 검색 지원:** Lucene 9.x부터 HNSW 기반 kNN 벡터 검색 내장

## 3. 작동 원리 (How it Works)
1. **인덱싱:** 문서 텍스트를 Analyzer로 토크나이징 → 역색인에 (단어, 문서ID, 위치) 기록
2. **검색:** 쿼리를 파싱하여 역색인 탐색 → BM25 스코어 계산 → 상위 결과 반환
3. **세그먼트 병합:** 소규모 세그먼트를 백그라운드에서 대형 세그먼트로 주기적 병합

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: [[Elasticsearch]]·Solr의 기반 라이브러리로 간접 활용
- 사례 2: 앱 내 임베디드 검색 엔진 (소규모 데이터셋)

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 검색 분야 수십 년 검증된 안정성
- 다양한 언어 Analyzer 지원 (한국어 등)
### 단점 (Cons)
- Java 라이브러리이므로 직접 사용 시 JVM 환경 필요
- 분산 처리는 직접 구현 필요 (→ [[Elasticsearch]]가 이를 해결)

## 6. 관련 기술 (Related Technologies)
- [[Elasticsearch]]
- [[BM25]]
- [[Sparse 임베딩]]
- [[Vector Search]]

## 7. 참고 자료 (References)
- Apache Lucene 공식 문서 (lucene.apache.org)
