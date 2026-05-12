---
tags: [technology, tech-note, ai-ml]
created: 2026-05-12
source_count: 0
status: stub
---

# Vector Search (벡터 검색)

## 1. 개요 (Overview)
- **기술명:** Vector Search (벡터 검색)
- **정의:** 쿼리 벡터와 저장된 벡터들 간의 수학적 유사도(코사인·내적·L2 거리)를 기반으로 가장 의미적으로 유사한 항목을 탐색하는 검색 방식
- **배경:** 키워드 기반 검색의 한계(동의어·오타·의미 불일치)를 극복하기 위해, [[임베딩]] 벡터 간 유사도를 활용하는 시맨틱 검색이 RAG 파이프라인의 핵심으로 자리잡았다.

## 2. 주요 특징 (Key Features)
- **시맨틱 검색:** 키워드 불일치에도 의미적으로 유사한 결과 반환
- **ANN (Approximate Nearest Neighbor):** 근사 탐색으로 속도와 정확도의 균형
- **HNSW 인덱스:** 계층적 그래프 구조로 빠른 ANN 탐색 (Faiss, Qdrant, Weaviate 등에서 사용)
- **하이브리드 검색:** [[BM25]] 키워드 검색과 벡터 검색을 결합하여 정확도 향상

## 3. 작동 원리 (How it Works)
1. 문서를 임베딩 모델로 벡터화 후 [[Vector Database]]에 저장·인덱싱
2. 쿼리 텍스트를 동일 임베딩 모델로 벡터화
3. 인덱스에서 쿼리 벡터와 가장 가까운 k개 벡터 탐색 (Top-k)
4. 결과를 [[Reranking]]으로 재정렬 후 LLM에 전달

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: RAG 파이프라인의 문서 검색 단계
- 사례 2: 이미지·영상 유사도 검색
- 사례 3: 추천 시스템 (사용자·아이템 벡터 매칭)

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 동의어·다국어·오타에 강한 의미 기반 검색
- 텍스트·이미지 등 멀티모달 검색 가능
### 단점 (Cons)
- 정확한 키워드 매칭이 필요한 경우 키워드 검색보다 부정확
- 벡터 인덱스 구축 및 전용 인프라 필요

## 6. 관련 기술 (Related Technologies)
- [[임베딩]]
- [[Vector Database]]
- [[Dense 임베딩]]
- [[BM25]]
- [[Reranking]]
- [[RAG]]
- [[VDPU]]

## 7. 참고 자료 (References)
- Johnson et al. (2019) "Billion-scale similarity search with GPUs" (Faiss)
