---
tags: [technology, tech-note, ai-ml]
created: 2026-05-12
source_count: 0
status: stub
---

# Sparse 임베딩 (Sparse Embedding)

## 1. 개요 (Overview)
- **기술명:** Sparse 임베딩 (Sparse Embedding, 희소 임베딩)
- **정의:** 어휘 크기(수만 차원)의 벡터 중 실제 등장한 단어에 해당하는 소수 차원만 0이 아닌 값을 가지는 희소(sparse) 표현 방식
- **배경:** TF-IDF·[[BM25]] 등 전통적 키워드 검색의 연장선으로, 신경망 기반의 가중치를 추가해 키워드 매칭 정확도를 높인 형태다. [[Dense 임베딩]]의 단점인 정확한 키워드 매칭 부재를 보완한다.

## 2. 주요 특징 (Key Features)
- **희소 벡터:** 전체 어휘 차원 중 극히 일부만 0이 아닌 값 (나머지는 0)
- **키워드 매칭 강점:** 특수 용어·고유명사·코드 등 정확한 단어 일치가 중요한 경우에 강함
- **신경망 기반 가중치:** 학습된 가중치로 단순 TF-IDF보다 더 정확한 단어 중요도 반영
- **대표 모델:** SPLADE, BM25+, ELSER (Elasticsearch Learned Sparse EncodeR)

## 3. 작동 원리 (How it Works)
- 입력 텍스트의 각 단어에 대해 어휘 사전 상의 가중치 계산
- 가중치가 0인 단어는 벡터에서 생략 → 희소성 유지
- 인덱싱 시 역색인(inverted index)과 호환 → 기존 검색 엔진([[Elasticsearch]], [[Lucene]])에서 활용 가능

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 법률·의료·특허 문서 검색 — 정확한 용어 일치 필수
- 사례 2: Dense 임베딩과 하이브리드 검색 구성하여 시맨틱 + 키워드 모두 커버

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 정확한 단어 매칭 — 전문 용어·고유명사·코드 검색에 강함
- 역색인 호환 — 기존 검색 엔진 인프라 재사용 가능
- 인덱스 크기가 Dense보다 작음 (희소성)
### 단점 (Cons)
- 동의어·다국어 처리에서 [[Dense 임베딩]]보다 취약
- 형태 변형(어근 처리)이 없으면 "run"과 "running" 별개로 처리

## 6. 관련 기술 (Related Technologies)
- [[임베딩]]
- [[Dense 임베딩]]
- [[BM25]]
- [[Vector Search]]
- [[Elasticsearch]]
- [[Reranking]]

## 7. 참고 자료 (References)
- Formal et al. (2021) "SPLADE: Sparse Lexical and Expansion Model"
