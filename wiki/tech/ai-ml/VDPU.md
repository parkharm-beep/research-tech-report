---
tags: [technology, tech-note, ai-ml]
created: 2026-05-07
source_count: 1
status: draft
source: [[raw/processed/"HBM, 터보퀀텀으론 해결 안되는 메모리 전쟁, 판을 바꾸는 반도체 VDPU가 온다"(정무경 디노티시아 대표).md]]
---

# VDPU (Vector Data Processing Unit)

## 1. 개요 (Overview)
- **기술명:** VDPU (Vector Data Processing Unit)
- **정의:** 벡터 데이터베이스의 유사도 검색(ANN Search)을 하드웨어 수준에서 가속하는 커스텀 반도체로, CPU의 벡터 검색 병목을 해소하기 위해 설계됨
- **배경:** 에이전틱 AI 확산으로 벡터 DB 검색 워크로드가 폭발적으로 증가하면서 CPU 단독으로는 처리 한계에 봉착. GPU는 LLM 추론에 점유되어 있어 전용 가속기 필요성이 대두됨. 디노티시아(Denotisia)가 2023년 10월 창업하여 개발 중인 개념.

## 2. 주요 특징 (Key Features)
- CPU 옆에 보조 프로세서로 장착되어 CPU의 벡터 검색 작업을 오프로드
- ANN(Approximate Nearest Neighbor) 알고리즘을 하드웨어로 직접 실행
- LLM 추론(GPU/NPU)과 독립적으로 동작하여 컴퓨팅 자원 경합 최소화
- GPU처럼 CPU의 지시를 받아 동작하는 코프로세서 구조
- 스토리지에서 핫 벡터를 메모리로 올리는 I/O 파이프라인도 관리

## 3. 작동 원리 (How it Works)
1. 사용자 질문 또는 에이전트 요청을 임베딩 모델이 벡터로 변환
2. CPU가 VDPU에 벡터 검색 명령 위임
3. VDPU가 벡터 DB의 HNSW 등 그래프 인덱스를 탐색하여 가장 가까운 벡터 후보 선별
4. 원본 문서 포인터를 CPU로 반환
5. CPU는 검색 결과를 LLM 컨텍스트에 조합하여 GPU로 추론 요청

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 에이전틱 RAG 솔루션 — 대규모 벡터 DB에서 실시간 멀티홉 검색을 VDPU가 가속 (Seahorse 제품)
- 사례 2: 클라우드 스토리지 시맨틱 인터페이스 — "내 데이터에서 X 찾아줘"를 자연어로 요청하면 VDPU가 전체 스토리지 벡터 검색
- 사례 3: 데이터센터 RAG 인프라 — 다수의 AI 서비스가 공유 벡터 DB를 조회할 때 VDPU로 처리량 확장

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- CPU 벡터 검색 병목을 해소하여 에이전트 루프 전체 latency 감소
- GPU 자원을 LLM 추론에만 집중시킬 수 있어 GPU 활용도 극대화
- 데이터 센터 전체 AI 워크로드 처리 효율 향상
### 단점 (Cons)
- 2026년 기준 아직 초기 스타트업(디노티시아) 단계로 상용화 전
- 빅테크가 자체 솔루션으로 충분히 대응 가능한 시장 규모일 경우 상업적 위험
- 벡터 DB 구조 변화(그래프 DB 혼용 등)에 따른 하드웨어 유연성 과제

## 6. 관련 기술 (Related Technologies)
- [[Vector Database]]
- [[Vector Search]]
- [[에이전틱 AI]]
- [[HBM]]
- [[KV 캐시]]
- [[RAG]]

## 7. 참고 자료 (References)
- [[raw/processed/"HBM, 터보퀀텀으론 해결 안되는 메모리 전쟁, 판을 바꾸는 반도체 VDPU가 온다"(정무경 디노티시아 대표).md]]
- https://www.youtube.com/watch?v=_TAjyEEuFv0
- 디노티시아 (Denotisia, "Deep Knowledge"): 2023년 창업, 2026년 Series A 900억 투자 유치
