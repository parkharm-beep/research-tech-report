---
tags: [technology, tech-note, data, ai-ml]
created: 2026-05-07
source_count: 1
status: draft
source: [[raw/processed/공공데이터의 인공지능 친화적 관리 핵심 요약 보고서.md]]
---

# Croissant

## 1. 개요 (Overview)
- **기술명:** Croissant (ML Commons Croissant)
- **정의:** AI 모델 및 학습 도구가 데이터셋을 즉시 처리할 수 있도록 규정한 데이터셋 메타데이터 표준
- **배경:** AI 학습 데이터의 출처·구조·품질·편향 정보가 표준화되지 않아 재사용성이 낮고, 편향 파악이 어려운 문제를 해결하기 위해 ML Commons가 제정

## 2. 주요 특징 (Key Features)
- 데이터셋의 구조(스키마)·생성 과정·어노테이션 기준·활용 제약을 기계가 읽을 수 있는 형태로 정의
- Hugging Face, Kaggle 등 주요 AI 데이터 플랫폼과 연동
- Schema.org 기반으로 웹 표준과 호환
- 편향(dataBiases)·한계(knownLimitations) 필드를 내장해 데이터 투명성 확보

## 3. 작동 원리 (How it Works)
- JSON-LD 형식으로 데이터셋에 대한 메타데이터를 정의
- AI 프레임워크(TensorFlow Datasets, PyTorch 등)가 Croissant 메타데이터를 파싱해 데이터를 자동으로 로드·전처리
- `rai:dataBiases`, `rai:knownLimitations` 등의 필드로 책임 있는 AI(RAI) 정보를 표준화

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 공공 AI 데이터셋 — 행안부 공공데이터 메타데이터 표준으로 채택(권고)
- 사례 2: AI 연구 재현성 확보 — 데이터셋 버전·생성 과정을 표준화하여 실험 재현 가능
- 사례 3: 데이터 카탈로그 연동 — DCAT·Dublin Core와 함께 국가 데이터 카탈로그와 연계

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- AI 도구가 데이터셋을 즉시 처리 가능 → 전처리 비용 절감
- 편향·한계 정보 표준화로 모델 신뢰성 향상
- 글로벌 AI 플랫폼과 호환

### 단점 (Cons)
- 표준 자체가 비교적 신규(2024~)로 도구·라이브러리 생태계 성숙 중
- 기존 데이터셋에 소급 적용 시 추가 작업 비용 발생

## 6. 관련 기술 (Related Technologies)
- [[Parquet]]
- [[온톨로지]]
- [[RDF]]

## 7. 참고 자료 (References)
- [[raw/processed/공공데이터의 인공지능 친화적 관리 핵심 요약 보고서.md]]
- 행정안전부, 「공공데이터의 인공지능 친화적 관리」, 2026-03-31
