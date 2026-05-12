---
tags: [technology, tech-note, ai-ml]
created: 2026-05-12
source_count: 0
status: stub
---

# Dense 임베딩 (Dense Embedding)

## 1. 개요 (Overview)
- **기술명:** Dense 임베딩 (Dense Embedding, 밀집 임베딩)
- **정의:** 트랜스포머 인코더로 생성된 고차원 실수 벡터로, 모든 차원에 0이 아닌 값이 분포하는 밀집(dense) 표현 방식
- **배경:** BERT 이후 문장 수준의 의미를 고차원 벡터로 표현하는 Dense Retrieval이 검색 분야에서 주류가 됐다. [[Sparse 임베딩]]과 대비되는 개념이다.

## 2. 주요 특징 (Key Features)
- **의미 기반 유사도:** 표면적 키워드가 달라도 의미가 비슷하면 벡터 거리가 가까움
- **밀집 벡터:** 768~4096 차원의 모든 위치에 실수값 존재 (Sparse와 달리 희소하지 않음)
- **Bi-Encoder 구조:** 쿼리와 문서를 독립적으로 인코딩 → 대규모 데이터셋에서 빠른 검색 가능
- **대표 모델:** text-embedding-ada-002, BGE, E5, Jina Embeddings, Voyage AI 등

## 3. 작동 원리 (How it Works)
1. 쿼리·문서를 각각 트랜스포머 인코더에 통과
2. [CLS] 토큰 또는 평균 풀링으로 문장 벡터 추출
3. 코사인 유사도로 쿼리-문서 벡터 간 유사도 계산
4. 유사도 기준으로 Top-k 결과 반환 ([[Vector Search]])

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: RAG 파이프라인의 시맨틱 문서 검색
- 사례 2: 이미지-텍스트 교차 검색 (CLIP 기반)
- 사례 3: 중복 문서 감지, 클러스터링

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 의미 기반 검색 — 동의어·다국어·오타에 강함
- 사전 인코딩으로 검색 시 빠른 응답
### 단점 (Cons)
- 정확한 키워드 매칭 능력이 [[Sparse 임베딩]]보다 낮음
- 도메인 특화 용어는 범용 임베딩 모델이 잘 못 다룰 수 있음

## 6. 관련 기술 (Related Technologies)
- [[임베딩]]
- [[Sparse 임베딩]]
- [[Vector Search]]
- [[Vector Database]]
- [[BM25]]
- [[Reranking]]

## 7. 참고 자료 (References)
- Karpukhin et al. (2020) "Dense Passage Retrieval for Open-Domain Question Answering"
