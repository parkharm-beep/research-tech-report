---
tags: [technology, tech-note, ai-ml]
created: 2026-05-07
source_count: 1
status: draft
source: [[raw/processed/GPT 못 쓰는 회사들, 우리 회사 맞춤 sLLM 이렇게 만드세요ㅣ비정형 자동화 실습 2시간 풀버전.md]]
---

# LoRA (Low-Rank Adaptation)

## 1. 개요 (Overview)
- **기술명:** LoRA (Low-Rank Adaptation, 저랭크 어댑테이션)
- **정의:** 대규모 언어 모델의 전체 가중치를 수정하지 않고, 각 레이어 앞에 저랭크 행렬 어댑터만 추가·학습시켜 특정 도메인이나 출력 형식에 특화시키는 파인튜닝 기법
- **배경:** 대형 LLM 전체를 파인튜닝하려면 수백 GB의 VRAM과 막대한 연산이 필요하다. LoRA는 학습 파라미터 수를 극적으로 줄여 일반 노트북이나 소형 GPU에서도 파인튜닝이 가능하게 한다.

## 2. 주요 특징 (Key Features)
- 원본 모델 가중치는 동결(freeze)하고, 추가된 저랭크 행렬(A, B)만 학습
- 학습 파라미터 수가 전체의 1% 미만으로 감소 → VRAM 절감
- 어댑터만 교체하면 동일 베이스 모델에 여러 특화 버전 적용 가능
- MLX (Apple Silicon용), Unsloth (NVIDIA용) 등의 프레임워크로 로컬 실행 지원
- 16GB 맥북에서 0.8B~8B 양자화 모델 파인튜닝 가능

## 3. 작동 원리 (How it Works)
1. 베이스 모델(예: Qwen 3.5 0.8B Instruct)을 로드하고 가중치를 동결
2. 각 Transformer 레이어에 저랭크 행렬 쌍(A: d×r, B: r×d, r≪d) 삽입
3. 학습 데이터(입출력 계약 쌍)로 어댑터 행렬만 업데이트
4. 추론 시 어댑터 출력이 원본 가중치 출력에 더해져 최종 응답 결정
5. 필요 시 어댑터 가중치만 저장·배포 (베이스 모델 공유)

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 비정형 데이터 구조화 — 리뷰·계약서 등을 특정 JSON 스키마로 추출하도록 특화
- 사례 2: 기업 내부 언어·형식 학습 — 사내 보고서 작성 스타일, 도메인 용어 적용
- 사례 3: 보안 민감 환경 — 외부 API 없이 로컬 sLLM에 LoRA 적용하여 온프레미스 운영

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 전체 파인튜닝 대비 VRAM·시간 비용 수십 배 절감
- 일반 노트북(16GB 맥북 등)에서도 파인튜닝 가능
- 어댑터만 교체하여 동일 베이스 모델의 다목적 활용
- 베이스 모델 훼손 없이 롤백·버전 관리 용이
### 단점 (Cons)
- 어댑터 랭크(r) 설정에 따라 성능-비용 트레이드오프 존재
- 데이터 품질이 낮으면 파인튜닝 효과 미미 (garbage in → garbage out)
- 일반 능력(General capability)이 저하될 수 있어 목적 외 작업에는 부적합

## 6. 관련 기술 (Related Technologies)
- [[sLLM]]
- [[양자화]]
- [[출력 계약]]
- [[LLM]]

## 7. 참고 자료 (References)
- [[raw/processed/GPT 못 쓰는 회사들, 우리 회사 맞춤 sLLM 이렇게 만드세요ㅣ비정형 자동화 실습 2시간 풀버전.md]]
- https://www.youtube.com/watch?v=WBQjc9V5C7Q
