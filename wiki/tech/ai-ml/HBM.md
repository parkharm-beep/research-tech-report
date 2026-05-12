---
tags: [technology, tech-note, ai-ml]
created: 2026-05-07
source_count: 1
status: draft
source: [[raw/processed/"HBM, 터보퀀텀으론 해결 안되는 메모리 전쟁, 판을 바꾸는 반도체 VDPU가 온다"(정무경 디노티시아 대표).md]]
---

# HBM (High Bandwidth Memory)

## 1. 개요 (Overview)
- **기술명:** HBM (High Bandwidth Memory, 고대역폭 메모리)
- **정의:** GPU/NPU에 직접 적층(스태킹)하여 초고속 데이터 전송을 제공하는 D램 기반 메모리로, LLM 추론 시 모델 파라미터와 KV 캐시를 저장하는 핵심 작업 메모리
- **배경:** 일반 DDR 메모리로는 GPU 연산 속도를 따라잡지 못하는 메모리 대역폭 한계 문제가 심화되면서, 다수의 D램 칩을 수직으로 쌓아 대역폭을 극대화한 HBM이 AI 가속기의 표준 메모리로 자리잡았다.

## 2. 주요 특징 (Key Features)
- 일반 GDDR 대비 수 배 이상의 메모리 대역폭 제공
- GPU 다이(die) 옆에 물리적으로 적층되어 초단거리 고속 접근 가능
- LLM 추론 시 **모델 파라미터** + **KV 캐시** 두 가지를 동시에 수용해야 함
- 컨텍스트 길이 증가, 배치 크기 증가 → KV 캐시 수요가 HBM 한계에 직접 충돌
- SK하이닉스, 삼성전자, Micron이 주요 공급사 (한국 메모리 기업의 핵심 경쟁 우위)

## 3. 작동 원리 (How it Works)
- LLM 추론 흐름에서 HBM의 역할:
  1. 모델 파라미터 전체를 HBM에 로드 (여러 사용자가 공유)
  2. 각 사용자 세션별 KV 캐시를 HBM에 별도 할당
  3. 매 토큰 생성 시 KV 캐시 전체를 읽어 attention 계산
  4. 세션 비활성 시 KV 캐시를 SSD로 스왑, 재활성 시 다시 HBM으로 로드
- HBM 용량 배분 예시: 100B 모델 파라미터(수백 GB) + 수백 사용자 × 수십 GB KV 캐시

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: AI 데이터센터 GPU 서버 — NVIDIA H100, A100 등 AI 가속기의 핵심 메모리
- 사례 2: 에이전틱 AI 인프라 — 에이전트가 유지하는 장기 컨텍스트의 실시간 저장소
- 사례 3: 벡터 DB 핫 데이터 — 자주 검색되는 벡터 인덱스를 HBM에 상주시켜 검색 속도 향상

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- GPU 연산 속도와 메모리 대역폭의 불균형을 해소
- 단위 면적당 최고 수준의 대역폭과 용량 밀도
- 한국 반도체 기업(SK하이닉스, 삼성)의 글로벌 경쟁 우위 분야
### 단점 (Cons)
- 에이전틱 AI 시대 데이터 증가 속도가 HBM 용량 확장 속도를 초과
- 단가가 매우 높아 AI 인프라 비용의 주요 병목
- Google TurboKontrol 등 KV 압축으로 일시 완화되어도 컨텍스트 확장 수요로 상쇄 — 메모리 수요 총량은 지속 증가

## 6. 관련 기술 (Related Technologies)
- [[KV 캐시]]
- [[VDPU]]
- [[LLM]]
- [[에이전틱 AI]]

## 7. 참고 자료 (References)
- [[raw/processed/"HBM, 터보퀀텀으론 해결 안되는 메모리 전쟁, 판을 바꾸는 반도체 VDPU가 온다"(정무경 디노티시아 대표).md]]
- https://www.youtube.com/watch?v=_TAjyEEuFv0
