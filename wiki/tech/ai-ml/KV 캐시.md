---
tags: [technology, tech-note, ai-ml]
created: 2026-05-07
source_count: 3
status: draft
source:
  - [[raw/processed/"HBM, 터보퀀텀으론 해결 안되는 메모리 전쟁, 판을 바꾸는 반도체 VDPU가 온다"(정무경 디노티시아 대표).md]]
  - [[raw/processed/칩 성능이 전부가 아니다_ NVIDIA가 AI 전쟁에서 절대 무너지지 않는 '3가지 소프트웨어 족쇄'.md]]
  - [[raw/processed/RAG 시대 끝나나...'1200만 토큰' 컨텍스트 창 제공하는 모델 출시.md]]
---

# KV 캐시 (Key-Value Cache)

## 1. 개요 (Overview)
- **기술명:** KV 캐시 (Key-Value Cache)
- **정의:** LLM이 토큰을 생성할 때 이전에 처리된 컨텍스트의 Key-Value 쌍을 GPU 메모리(HBM)에 저장해두는 버퍼로, 매 토큰 생성 시 재계산 없이 참조하는 작업 메모리
- **배경:** Transformer 구조에서 각 토큰을 생성할 때마다 이전 모든 토큰의 attention 연산이 필요하다. 이를 매번 재계산하면 비용이 폭증하므로 중간 결과(Key, Value)를 메모리에 저장해두는 캐싱 구조가 도입됐다.

## 2. 주요 특징 (Key Features)
- 현재 세션 내 모든 컨텍스트를 Key-Value 행렬 형태로 HBM에 저장
- 컨텍스트 길이(token 수)에 비례해 용량이 선형 증가
- 100B 이하 모델 기준 128K context → 수십 GB 규모의 KV 캐시 발생
- 여러 사용자가 GPU 리소스를 공유하는 클라우드 환경에서 용량 경합 심화
- 세션 유휴 시 스토리지로 스왑(offload)되었다가 재사용 시 다시 올라오는 I/O 병목 발생

## 3. 작동 원리 (How it Works)
1. 사용자 질문 + 검색된 문서 등이 컨텍스트로 LLM에 입력됨
2. LLM은 입력 전체를 신경망을 통해 처리하여 Key-Value 행렬 계산
3. 해당 KV 행렬을 HBM(GPU 고대역폭 메모리)에 저장
4. 이후 토큰 하나씩 생성할 때마다 저장된 KV 캐시를 읽어 attention 연산 수행
5. 세션 간 GPU 리소스 공유를 위해 비활성 KV 캐시는 SSD/NVMe로 오프로드

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 긴 문서 기반 Q&A — 수십 페이지 문서를 한 번 KV로 저장 후 다양한 질문에 재활용
- 사례 2: 에이전트 시스템 — 에이전트가 누적한 작업 이력(컨텍스트)을 KV 캐시로 유지하며 반복 추론

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 매 토큰 생성 시 전체 컨텍스트 재계산 불필요 → 추론 속도 대폭 향상
- 컨텍스트 길이가 길수록 재계산 절감 효과 극대화
### 단점 (Cons)
- HBM 용량의 상당 부분을 점유하여 배치 처리(batch size) 제한
- 클라우드 멀티테넌트 환경에서 세션별 KV 캐시 할당 → HBM 압박 심화
- 스왑 I/O 발생 시 latency 급증
- Google TurboKontrol(터보퀀텀) 등 KV 압축 기술은 용량 절감 효과가 있으나, 절감된 용량만큼 컨텍스트를 더 늘려쓰기 때문에 메모리 수요 총량은 줄지 않음

## 6. 관련 기술 (Related Technologies)
- [[HBM]]
- [[LLM]]
- [[에이전틱 AI]]
- [[Prompt Caching]]
- [[VDPU]]
- [[CUDA]]
- [[희소 어텐션]]

## 7. 참고 자료 (References)
- [[raw/processed/"HBM, 터보퀀텀으론 해결 안되는 메모리 전쟁, 판을 바꾸는 반도체 VDPU가 온다"(정무경 디노티시아 대표).md]]
- [[raw/processed/칩 성능이 전부가 아니다_ NVIDIA가 AI 전쟁에서 절대 무너지지 않는 '3가지 소프트웨어 족쇄'.md]]
- https://www.youtube.com/watch?v=_TAjyEEuFv0

---

> [!note] 2026-05-07 소스 추가: 희소 어텐션 구조의 KV 캐시 절감
> [[희소 어텐션]] 기반의 SubQ 1M-Preview 모델은 1200만 토큰 처리 시 기존 트랜스포머 대비 KV 캐시 메모리를 수백분의 1 수준으로 절감했다고 발표(독립 검증 진행 중). 풀 어텐션 구조와 달리 의미적으로 중요한 토큰 쌍만 선택 계산하여 KV 상태를 대폭 압축한다. HBM 증설 없이 아키텍처 수준에서 초장문 컨텍스트를 처리하는 방향성으로, VDPU 같은 하드웨어 가속과는 상보적 접근이다.

> [!note] 2026-05-07 소스 추가: KV 캐시 최적화 오픈소스 생태계
> NVIDIA CUDA의 구조적 종속 문제를 완화하는 오픈소스 추론 엔진들이 KV 캐시 계층에서 균열을 내고 있음. **vLLM**, **SGLang**, **LMCache** 등이 KV 캐시 최적화와 오프로딩 기능을 제공하며, CUDA 종속을 유지하면서도 운영 효율을 높이는 방향으로 발전 중. 이는 NVIDIA 족쇄의 완전한 해소가 아닌 그 위에서 효율화하는 계층이다.
