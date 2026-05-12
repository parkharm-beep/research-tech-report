---
tags: [technology, tech-note, data]
created: 2026-05-07
source_count: 1
status: draft
source: [[raw/processed/공공데이터의 인공지능 친화적 관리 핵심 요약 보고서.md]]
---

# Parquet

## 1. 개요 (Overview)
- **기술명:** Apache Parquet
- **정의:** 열(Column) 단위로 데이터를 저장하는 오픈소스 열 지향(Columnar) 파일 포맷
- **배경:** 행 기반 CSV/JSON은 특정 컬럼만 조회할 때도 전체 행을 읽어야 해 대용량 데이터 처리에 비효율적. Parquet는 필요한 컬럼만 선택적으로 읽을 수 있어 I/O 부하와 클라우드 비용을 획기적으로 절감

## 2. 주요 특징 (Key Features)
- **열 지향 저장**: 특정 피처(Column)만 선택적 접근 가능 → 불필요한 데이터 읽기 제거
- **압축 효율 우수**: 동일 타입 데이터가 연속 저장되어 압축률이 행 기반 대비 높음
- **스키마 내장**: 컬럼명·타입 정보를 파일 자체에 포함
- **Apache 생태계 호환**: Spark, Pandas, Arrow, DuckDB 등과 네이티브 연동

## 3. 작동 원리 (How it Works)
- 데이터를 행 그룹(Row Group)으로 분할하고, 각 행 그룹 내 데이터를 컬럼별로 연속 저장
- 각 컬럼에 독립적인 압축(Snappy, Gzip, Zstd 등) 적용
- 쿼리 시 필요한 컬럼의 Row Group만 읽어 I/O 최소화 (Predicate Pushdown)

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: AI/ML 학습 데이터 파이프라인 — 대용량 피처 데이터를 효율적으로 로드
- 사례 2: 공공데이터 개방 포맷 — 행안부 AI 친화적 공공데이터 권장 포맷
- 사례 3: 데이터 레이크하우스 — Delta Lake, Apache Iceberg의 기반 파일 포맷

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 컬럼 선택적 읽기로 I/O 및 클라우드 비용 절감
- 높은 압축률로 저장 비용 절감
- GPU 서버·클라우드 환경에서 자동 처리 친화적

### 단점 (Cons)
- 행 단위 추가(append)가 느림 → 실시간 트랜잭션보다 배치 처리에 적합
- 텍스트 편집기로 직접 열람 불가 (바이너리 포맷)
- 소규모 데이터에서는 CSV 대비 오버헤드 발생

## 6. 관련 기술 (Related Technologies)
- [[Croissant]]
- [[Vector Database]]
- [[그래프DB]]

## 7. 참고 자료 (References)
- [[raw/processed/공공데이터의 인공지능 친화적 관리 핵심 요약 보고서.md]]
- 행정안전부, 「공공데이터의 인공지능 친화적 관리」, 2026-03-31
