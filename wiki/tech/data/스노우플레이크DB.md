---
tags: [technology, tech-note, data]
created: 2026-05-12
source_count: 0
status: stub
---

# Snowflake (스노우플레이크)

## 1. 개요 (Overview)
- **기술명:** Snowflake (스노우플레이크 DB)
- **정의:** 스토리지와 컴퓨팅을 분리한 클라우드 네이티브 데이터 웨어하우스 플랫폼으로, AWS·GCP·Azure 위에서 멀티클라우드 데이터 분석을 지원한다
- **배경:** 2012년 설립된 Snowflake Inc.가 개발했다. 온프레미스 데이터 웨어하우스(Teradata·Vertica)의 고비용·관리 부담을 해소하기 위해 클라우드 퍼스트로 설계됐다.

## 2. 주요 특징 (Key Features)
- **컴퓨팅·스토리지 분리:** 워크로드별 독립 컴퓨팅(Virtual Warehouse) — 비용 최적화
- **공유 데이터 (Data Sharing):** 데이터를 복사 없이 다른 Snowflake 계정과 실시간 공유
- **타임 트래블 (Time Travel):** 최대 90일 이내 과거 데이터 버전 조회·복구
- **Zero-Copy 클론:** 데이터 실제 복사 없이 논리적 복제본 생성
- **멀티클라우드:** AWS·GCP·Azure에서 동일한 인터페이스로 운영

## 3. 작동 원리 (How it Works)
- 데이터를 Micro-partition으로 분할하여 오브젝트 스토리지(S3 등)에 컬럼 형식(Parquet 유사)으로 저장
- Virtual Warehouse(컴퓨팅 클러스터)가 쿼리 실행 시에만 가동 → 미사용 시 비용 없음
- SQL 표준 지원 + 반정형 데이터(JSON, Parquet) 네이티브 처리

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 기업 데이터 웨어하우스 — 다양한 소스 데이터를 중앙화하여 BI 분석
- 사례 2: 데이터 마켓플레이스 — 써드파티 데이터를 공유·구매
- 사례 3: AI/ML 파이프라인 — 학습 데이터 저장·전처리 (Snowpark ML 지원)

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 인프라 관리 없이 완전 관리형(SaaS) 운영
- 사용한 컴퓨팅만 과금 — 변동 워크로드에 효율적
### 단점 (Cons)
- 클라우드 종속 — 온프레미스 배포 불가
- 대규모 사용 시 비용이 빠르게 증가 (Virtual Warehouse 크기·시간 기준 과금)

## 6. 관련 기술 (Related Technologies)
- [[그래프DB]]
- [[Vector Database]]

## 7. 참고 자료 (References)
- Snowflake 공식 문서 (docs.snowflake.com)
