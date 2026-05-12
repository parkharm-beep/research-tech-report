---
tags: [technology, tech-note, ai-ml]
created: 2026-05-12
source_count: 0
status: stub
---

# Reranking (리랭킹)

## 1. 개요 (Overview)
- **기술명:** Reranking (리랭킹, 재순위화)
- **정의:** [[Vector Search]] 또는 키워드 검색으로 1차 후보 문서를 추출한 후, 더 정교한 Cross-Encoder 모델로 쿼리-문서 쌍을 재평가하여 최종 순위를 재조정하는 2단계 검색 기법
- **배경:** Bi-Encoder([[Dense 임베딩]]) 기반 검색은 속도가 빠르지만 정확도가 낮을 수 있다. Reranking으로 정확도를 높이면서 전체 후보를 Cross-Encoder로 비교하는 비용을 줄이는 투-스테이지 파이프라인이 RAG의 표준으로 자리잡았다.

## 2. 주요 특징 (Key Features)
- **Cross-Encoder 방식:** 쿼리와 문서를 함께 입력하여 상호작용 학습 → 정확도 높음
- **2단계 파이프라인:** 1단계 빠른 검색(Recall) → 2단계 정밀 Reranking(Precision)
- **대표 모델:** Cohere Rerank, BGE-Reranker, Jina Reranker, Cross-Encoder (sentence-transformers)

## 3. 작동 원리 (How it Works)
1. 1단계: [[Vector Search]] 또는 [[BM25]]로 Top-100 후보 문서 빠르게 검색
2. 2단계: 각 (쿼리, 문서) 쌍을 Cross-Encoder에 입력하여 관련성 점수 계산
3. 점수 기준으로 재정렬 → 상위 k개(예: Top-5)를 LLM에 전달

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: RAG 파이프라인에서 1차 검색 결과의 품질 향상
- 사례 2: 검색 엔진(Elasticsearch)의 결과를 LLM 기반으로 재정렬

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 검색 정확도 대폭 향상 (쿼리-문서 상호작용 반영)
- 1단계 검색이 Recall 확보, 2단계가 Precision 보장
### 단점 (Cons)
- Cross-Encoder는 쌍별 비교로 대용량 후보 처리 시 느림
- API 비용 추가 발생 (Cohere Rerank 등)

## 6. 관련 기술 (Related Technologies)
- [[Vector Search]]
- [[Dense 임베딩]]
- [[BM25]]
- [[RAG]]
- [[청킹]]

## 7. 참고 자료 (References)
- Nogueira & Cho (2019) "Passage Re-ranking with BERT"
