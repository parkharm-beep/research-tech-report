---
tags: [technology, tech-note, ai-ml]
created: 2026-05-12
source_count: 0
status: stub
---

# VLM (Vision Language Model)

## 1. 개요 (Overview)
- **기술명:** VLM (Vision Language Model, 비전-언어 모델)
- **정의:** 이미지와 텍스트를 동시에 입력받아 이해·생성하는 멀티모달 대형 언어 모델
- **배경:** 텍스트만 처리하는 [[LLM]]의 한계를 극복하기 위해, 비전 인코더(ViT 등)와 언어 모델을 결합한 멀티모달 아키텍처가 등장했다. GPT-4V, Claude 3, Gemini, LLaVA 등이 대표적이다.

## 2. 주요 특징 (Key Features)
- **멀티모달 입력:** 이미지+텍스트를 함께 입력하여 이미지에 대한 질문·분석·설명 가능
- **비전 인코더 통합:** CLIP·ViT 등의 이미지 인코더를 언어 모델과 연결
- **다양한 비전 태스크:** 이미지 캡셔닝, 시각적 Q&A, OCR, 차트 해석, 객체 감지

## 3. 작동 원리 (How it Works)
1. 이미지를 비전 인코더(예: ViT)로 패치 단위 벡터 시퀀스로 변환
2. 이미지 벡터를 언어 모델의 토큰 공간으로 프로젝션
3. 텍스트 토큰과 이미지 토큰을 함께 트랜스포머에 입력
4. 언어 모델 디코더가 텍스트 응답 생성

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 의료 이미지 분석 (X-ray·MRI 설명 생성)
- 사례 2: 문서 이미지의 표·차트 해석 및 데이터 추출
- 사례 3: UI 스크린샷을 입력받아 코드 생성 (스크린샷 → React 컴포넌트)
- 사례 4: 제조 공정 이미지 기반 불량 감지

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 이미지+텍스트 혼합 입력으로 기존 텍스트 전용 모델이 처리 못하던 태스크 수행
- 별도 컴퓨터 비전 파이프라인 없이 단일 모델로 처리
### 단점 (Cons)
- 이미지 해상도·복잡도에 따라 성능 편차
- 정밀한 공간적 추론(정확한 좌표, 픽셀 단위 분할)은 여전히 어려움

## 6. 관련 기술 (Related Technologies)
- [[LLM]]
- [[임베딩]]
- [[에이전틱 AI]]

## 7. 참고 자료 (References)
- OpenAI GPT-4V Technical Report (2023)
- Liu et al. (2023) "LLaVA: Visual Instruction Tuning"
