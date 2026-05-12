---
tags: [technology, tech-note, AI, RAG]
created: 2026-04-22
status: draft
---

# Contextual Retrieval

## 1. 개요 (Overview)
- **기술명:** Contextual Retrieval (맥락적 검색)
- **정의:** RAG(Retrieval-Augmented Generation) 시스템에서 문서를 작은 단위(Chunk)로 나눌 때 유실되는 맥락을 보존하기 위해, 각 청크 앞에 전체 문서의 관련 맥락을 요약하여 추가하는 기술입니다.
- **배경:** 기존 RAG는 대규모 문서를 처리하기 위해 텍스트를 잘게 나누는데, 이 과정에서 특정 청크가 어떤 문서나 주제에 속하는지 등의 '맥락'이 사라져 검색 엔진이 관련 정보를 찾지 못하는(Retrieval Failure) 문제가 발생합니다.

## 2. 주요 특징 (Key Features)
- **Contextual Embeddings:** 청크에 맥락 정보가 포함된 상태로 벡터화되어 의미적 검색의 정밀도가 향상됩니다.
- **Contextual BM25:** 키워드 검색 시에도 맥락에 포함된 주요 용어(기업명, 날짜 등)가 반영되어 정확도가 높아집니다.
- **Hybrid Search 결합:** 벡터 검색(Dense)과 키워드 검색(Sparse)의 장점을 모두 활용합니다.
- **Prompt Caching 활용:** Anthropic의 프롬프트 캐싱 기술을 통해 맥락 생성 비용을 최대 90%까지 절감할 수 있습니다.

## 3. 작동 원리 (How it Works)
1. **맥락 생성:** LLM(예: Claude 3 Haiku)이 전체 문서와 개별 청크를 읽고, 해당 청크를 설명하는 짧은(50~100토큰) 맥락 문장을 생성합니다.
2. **청크 보강 (Augmentation):** 생성된 맥락 문장을 원래의 청크 텍스트 앞에 붙입니다. (예: "이 청크는 ACME Corp의 2023년 2분기 실적 보고서에서 발췌됨; 매출이 3% 성장함")
3. **인덱싱:** 보강된 청크를 임베딩 모델을 통해 벡터 데이터베이스에 저장하고, 동시에 BM25 인덱스를 생성합니다.
4. **검색 및 재정렬:** 사용자 쿼리에 대해 하이브리드 검색을 수행한 후, Reranker를 통해 최종 후보를 선별합니다.

## 4. 주요 활용 사례 (Use Cases)
- **사내 지식 베이스:** 복잡한 규정이나 매뉴얼에서 정확한 조항을 찾아야 하는 경우.
- **금융/법률 문서 분석:** 수많은 보고서 중 특정 시점이나 업체의 데이터를 정확히 추출해야 할 때.
- **고객 지원 자동화:** 제품명이나 버전 정보가 생략된 상담 기록에서 정확한 해결책을 검색할 때.

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 검색 실패율(Top-20 failure rate)을 최대 67%까지 감소시킵니다.
- 모호한 대명사나 생략된 주어를 맥락으로 보충하여 검색 성능을 극대화합니다.
- 프롬프트 캐싱을 지원하는 모델 사용 시 매우 경제적인 비용으로 구현 가능합니다.
### 단점 (Cons)
- 데이터 인덱싱 단계에서 LLM을 통한 전처리 과정이 추가되어 시간이 소요됩니다.
- 모든 청크에 대해 맥락을 생성해야 하므로 초기 구축 비용이 발생합니다.

## 6. 관련 기술 (Related Technologies)
- [[RAG]] (Retrieval-Augmented Generation)
- [[BM25]]
- [[Vector Database]]
- [[Prompt Caching]]
- [[Reranking]]

## 7. 참고 자료 (References)
- [Anthropic: Introducing Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval)
- [Improve RAG Performance with Contextual Retrieval](https://plainenglish.io/blog/contextual-retrieval-anthropic)
