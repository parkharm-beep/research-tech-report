# CLAUDE.md — LLM 위키 관리 워크플로우

## 개요

이 CLAUDE.md는 [Karpathy의 llm-wiki 패턴](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)을 기반으로
Obsidian Vault를 운영하기 위한 Claude Code 워크플로우를 정의합니다.

**핵심 원칙**
- `raw/` 소스는 **불변(immutable)** — LLM은 읽기만 하고 절대 수정하지 않는다
- `wiki/`는 **LLM이 쓰고 유지** — 사람은 읽기만 한다
- 지식은 매 질문마다 재발견하지 않고 **위키에 누적**된다

**세 가지 핵심 오퍼레이션:**
| 오퍼레이션 | 트리거 | 목적 |
|---|---|---|
| **Ingest** | `raw/`에 새 파일 추가 | 소스 처리 후 위키 전반 업데이트 |
| **Query** | 사용자 질문 | 위키 검색 → 합성 → 가치 있는 답변은 위키 파일링 |
| **Lint** | 주기적 요청 | 모순·고아 페이지·누락 링크 탐지, 명백한 문제 자동 수정·나머지 보고 |

---

## 디렉토리 구조

```
옵시디언/
├── raw/                        ← 원본 파일 (절대 수정 금지)
│   ├── assets/                 ← 이미지 로컬 저장 (Obsidian Web Clipper 연동)
│   ├── processed/              ← Ingest 완료된 파일
│   └── errors/                 ← 읽기 실패 파일
├── Templates/
│   ├── 기술 정리 템플릿.md      ← 기술자료용
│   ├── 보고서 정리 템플릿.md    ← 보고서용 (신규)
│   └── 가이드라인 정리 템플릿.md ← 가이드라인용 (신규)
├── wiki/
│   ├── tech/                   ← 기술 위키 (LLM이 생성·유지)
│   │   ├── frontend/
│   │   ├── backend/
│   │   ├── infrastructure/
│   │   ├── data/
│   │   ├── ai-ml/
│   │   ├── security/
│   │   ├── mobile/
│   │   ├── concepts/           ← 카테고리 횡단 개념 페이지
│   │   └── etc/
│   ├── work/                   ← 업무 문서 위키 (보고서·가이드라인) (신규)
│   │   ├── reports/            ← 보고서
│   │   └── guidelines/         ← 가이드라인
│   ├── _index.md               ← 전체 위키 카탈로그 (링크 + 한 줄 요약 + 날짜)
│   └── _log.md                 ← 작업 이력 append-only
└── CLAUDE.md
```

---

## 오퍼레이션 1: Ingest

`raw/`에 새 파일이 추가될 때 실행한다.

### 1단계: raw/ 폴더 스캔

- 지원 형식: `.md`, `.txt`, `.pdf`, `.html`, `.docx`
- `raw/processed/`에 있는 파일은 이미 처리됨 → 스킵
- `raw/assets/`는 이미지 저장 전용 폴더 → 스킵 (Ingest 대상 아님)

### 2단계: 소스 읽기 및 파일 유형·카테고리 판별

각 파일에 대해:
1. 파일 전문(full text) 읽기
2. **파일 유형 판별** (아래 기준 적용)
3. 핵심 기술 키워드 추출 및 기술 카테고리 판별
4. **기존 wiki/ 페이지 스캔** — 이 소스와 관련된 기존 노트 식별

**파일 유형 판별 기준:**

| 유형 | 판별 기준 | 템플릿 | 저장 위치 |
|---|---|---|---|
| **기술자료** | 특정 기술 개념 설명, 튜토리얼, 기술 뉴스·분석, 제품 소개 | `Templates/기술 정리 템플릿.md` | `wiki/tech/{카테고리}/` |
| **보고서** | 연구보고서, 시장조사, 정책 보고서, 분석 리포트, 백서 | `Templates/보고서 정리 템플릿.md` | `wiki/work/reports/` |
| **가이드라인** | 지침서, 표준, 프레임워크, 규정, 매뉴얼, 권고안 | `Templates/가이드라인 정리 템플릿.md` | `wiki/work/guidelines/` |

> 판별이 모호한 경우 내용의 주요 목적(기술 설명 vs 업무 활용)을 기준으로 결정한다.

**기술 카테고리 분류 기준 (기술자료 및 기술 요소 추출 시 공통 적용):**

| 카테고리 | 폴더명 | 키워드 예시 |
|---|---|---|
| 프론트엔드 | `frontend/` | React, Vue, Next.js, CSS, TypeScript, Svelte, Webpack |
| 백엔드 | `backend/` | Node.js, Python, FastAPI, Spring, REST API, GraphQL, gRPC |
| 인프라/DevOps | `infrastructure/` | Docker, Kubernetes, Terraform, CI/CD, AWS, GCP, Azure, Nginx |
| 데이터 | `data/` | SQL, PostgreSQL, MongoDB, Spark, Kafka, dbt, Airflow, ETL |
| AI/ML | `ai-ml/` | LLM, RAG, PyTorch, TensorFlow, Embedding, Fine-tuning, Agent |
| 보안 | `security/` | OAuth, JWT, Zero Trust, SAST, DAST, 취약점, 암호화 |
| 모바일 | `mobile/` | iOS, Android, Swift, Kotlin, React Native, Flutter |
| 개념 | `concepts/` | 여러 카테고리에 걸쳐 있는 횡단 개념 |
| 기타 | `etc/` | 위 카테고리에 해당하지 않는 경우 |

> 하나의 파일이 여러 카테고리에 해당하면 **모든 해당 카테고리 폴더**에 노트를 생성하고, `_index.md`에도 각 카테고리 섹션에 모두 등록한다.

### 3단계: 위키 페이지 생성 및 기존 페이지 업데이트

> **단일 소스가 10-15개 페이지에 영향을 줄 수 있다.** 신규 페이지 생성에 그치지 말고 기존 관련 페이지도 보완한다.

#### 3-A. 기술자료 처리 (`Templates/기술 정리 템플릿.md` 적용)

- 파일명: `{기술명}.md`
- 경로: `wiki/tech/{카테고리}/{기술명}.md`
- `{{date}}`는 파일 생성 당일 날짜(`YYYY-MM-DD`)로, `{{title}}`은 기술명으로 직접 치환

```markdown
---
tags: [technology, tech-note, {카테고리태그}]
created: {YYYY-MM-DD}
source_count: 1
status: draft
source: [[raw/processed/{원본파일명}]]
---

# {기술명}

## 1. 개요 (Overview)
- **기술명:** {기술명}
- **정의:** {원문을 바탕으로 이 기술이 무엇인지 한 문장으로 정의}
- **배경:** {이 기술이 등장하게 된 배경이나 해결하고자 하는 문제}

## 2. 주요 특징 (Key Features)
- {원문에서 추출한 특징 1}
- {원문에서 추출한 특징 2}
- {원문에서 추출한 특징 3}

## 3. 작동 원리 (How it Works)
- {기술의 핵심 메커니즘이나 아키텍처 설명}

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: {원문 기반}
- 사례 2: {원문 기반}

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- {원문에서 언급된 장점}
### 단점 (Cons)
- {원문에서 언급된 단점, 없으면 일반적으로 알려진 단점}

## 6. 관련 기술 (Related Technologies)
- [[{연관기술1}]]
- [[{연관기술2}]]

## 7. 참고 자료 (References)
- [[raw/processed/{원본파일명}]]
- {원문에 URL이 있으면 포함}
```

#### 3-B. 보고서 처리 (`Templates/보고서 정리 템플릿.md` 적용)

- 파일명: `{보고서 제목}.md`
- 경로: `wiki/work/reports/{보고서 제목}.md`
- `Templates/보고서 정리 템플릿.md`의 섹션 구조를 준수하여 내용을 채운다
- frontmatter 필수 필드:
  ```yaml
  tags: [work, report, {주제태그}]
  created: {YYYY-MM-DD}
  source_count: 1
  status: draft
  doc_type: report
  source: [[raw/processed/{원본파일명}]]
  ```
- 핵심 요약, 상세 내용(현황·분석 결과), 시사점을 중심으로 정리한다
- 관련 기술 요소는 아래 3-D 규칙에 따라 별도 추출하여 `wiki/tech/`에 생성 후 링크한다

#### 3-C. 가이드라인 처리 (`Templates/가이드라인 정리 템플릿.md` 적용)

- 파일명: `{가이드라인 제목}.md`
- 경로: `wiki/work/guidelines/{가이드라인 제목}.md`
- `Templates/가이드라인 정리 템플릿.md`의 섹션 구조를 준수하여 내용을 채운다
- frontmatter 필수 필드:
  ```yaml
  tags: [work, guideline, {주제태그}]
  created: {YYYY-MM-DD}
  source_count: 1
  status: draft
  doc_type: guideline
  source: [[raw/processed/{원본파일명}]]
  ```
- 핵심 원칙, 요구사항/권고사항, 적용 방법을 중심으로 정리한다
- 관련 기술 요소는 아래 3-D 규칙에 따라 별도 추출하여 `wiki/tech/`에 생성 후 링크한다

#### 3-D. 보고서·가이드라인에서 기술 요소 추출 (공통)

보고서 또는 가이드라인을 처리한 후, 문서에서 언급된 기술 개념·도구·방법론이 있으면 **추가로** `Templates/기술 정리 템플릿.md`를 적용하여 `wiki/tech/{카테고리}/`에 기술 페이지를 생성한다.

- 추출 기준: 특정 기술 용어, 플랫폼, 프레임워크, 알고리즘 등 독립 기술 항목으로 분리 가능한 요소
- 마케팅 문구나 단순 언급은 추출하지 않는다
- 이미 `wiki/tech/`에 해당 페이지가 있으면 신규 생성 대신 기존 페이지를 업데이트한다
- 생성된 기술 페이지는 보고서/가이드라인 페이지의 "5. 관련 기술 요소" 섹션에 링크한다

#### 3-E. 기존 페이지 업데이트 규칙 (공통)

- 새 소스에서 기존 페이지의 내용을 보완하는 정보가 있으면 해당 섹션을 추가·수정한다
- 기존 주장과 모순되면 해당 항목에 `> [!note] {날짜} 소스 추가: ...` 형식으로 표기한다
- frontmatter의 `source_count`를 1 증가시키고, `source` 항목에 `[[raw/processed/{원본파일명}]]` 형식으로 새 소스를 추가한다
- **`_index.md` 확인 상태 갱신:** 기존 페이지를 업데이트할 때, `_index.md`에서 해당 항목 줄 끝에 `✅`가 있으면 `🔄`로 교체한다 (사용자가 확인한 내용이 변경됐음을 알리기 위함)

**파일 충돌 처리:**

기존 페이지가 있을 때 두 가지 경우를 구분한다:

- **같은 기술·주제를 다루는 경우** → 신규 파일 생성 없이 기존 페이지를 업데이트 (3-E 규칙 적용)
- **다른 기술·주제인데 이름만 같은 경우** → `{파일명}_v2.md` 생성. 이후 `_v3`, `_v4` 순으로 증가하고, 두 페이지 모두 상단에 `> [!note] 동명 페이지: [[{파일명}]] 참조` 표기

### 4단계: _index.md 업데이트

`wiki/_index.md`에 새 페이지를 추가한다. 형식은 **카테고리별 테이블 (페이지 | 설명 | 업데이트 | 확인)**:

```markdown
# 기술 위키 인덱스

> 마지막 업데이트: {YYYY-MM-DD}
> 총 기술 노트: {N}개
> 확인 상태: ✅ 확인 | 🔄 갱신됨(재확인 필요) | (없음) 미확인

## 🖥️ Frontend

| 페이지 | 설명 | 업데이트 | 확인 |
|---|---|---|---|
| [[React]] | 컴포넌트 기반 UI 라이브러리, 선언적 렌더링 | 2026-01-15 | |

## ⚙️ Backend
## ☁️ Infrastructure
## 🗄️ Data
## 🤖 AI / ML
## 🔒 Security
## 📱 Mobile
## 🧩 Concepts
## 📦 기타

---

## 📋 Reports (보고서)

| 페이지 | 설명 | 업데이트 | 확인 |
|---|---|---|---|
| [[{보고서 제목}]] | {한 줄 요약} | {날짜} | |

## 📌 Guidelines (가이드라인)
- [[{가이드라인 제목}]] — {한 줄 요약} ({날짜})
```

### 5단계: _log.md 업데이트

`wiki/_log.md`에 처리 내역을 **append**한다 (절대 기존 내용 삭제 금지):

```markdown
## [{YYYY-MM-DD}] ingest | {원본 파일명}
- 생성: {생성된 페이지 목록}
- 업데이트: {업데이트된 기존 페이지 목록}
- 스킵: {이유가 있으면 기록}
```

### 6단계: 원본 파일 이동

처리 완료된 파일은 `raw/processed/`로 이동한다.

> 3단계에서 소스 링크를 `[[raw/processed/{원본파일명}]]`으로 작성하므로, 이동 후에도 링크가 유효하다. 이동 전에 위키 파일을 수정할 필요 없다.

---

## 오퍼레이션 2: Query

사용자가 기술 관련 질문을 할 때 실행한다.

### 처리 흐름

1. `wiki/_index.md` 읽기 → 관련 페이지 식별
2. 관련 페이지들 읽기
3. 합성 답변 작성 (인용 포함)
4. **가치 있는 답변은 위키에 파일링** — 비교표, 분석, 발견한 연결고리 등

### 파일링 기준

다음에 해당하면 `wiki/tech/{카테고리}/{제목}.md`로 저장한다:
- 두 개 이상의 기술을 비교·대조한 답변
- 여러 페이지를 종합해 도출한 새로운 인사이트
- 자주 반복될 것 같은 질문에 대한 답변

파일링 시 frontmatter에 `status: query-derived` 태그를 추가한다.

새 파일이 생성된 경우 `wiki/_index.md`에도 해당 카테고리 섹션에 추가한다.

### _log.md 기록

```markdown
## [{YYYY-MM-DD}] query | {질문 요약}
- 참조 페이지: {읽은 페이지 목록}
- 파일링: {새로 생성된 페이지, 없으면 생략}
```

---

## 오퍼레이션 3: Lint

사용자가 "위키 점검해줘" 또는 "lint 실행해줘"를 요청할 때 실행한다.

- 점검 범위: `wiki/` 전체 (`wiki/tech/`, `wiki/work/reports/`, `wiki/work/guidelines/` 포함)
- 수정 방침: 명백한 오타·깨진 링크·빈 섹션은 자동 수정한다. 모순·중복처럼 판단이 필요한 항목은 수정하지 않고 "권장 조치"로 보고한 뒤 사용자 확인 후 처리한다.

### 점검 항목

1. **모순 탐지** — 동일 기술에 대해 서로 상충하는 주장이 있는 페이지
2. **고아 페이지(orphan)** — inbound 링크가 없는 페이지
3. **미완성 링크** — `[[기술명]]`으로 언급되었지만 해당 페이지가 없는 경우
4. **중복 페이지** — 실질적으로 같은 기술을 다루는 복수의 페이지
5. **stale 정보** — 더 최신 소스가 추가된 후 업데이트되지 않은 오래된 주장
6. **빈 섹션** — `(정보 없음)` 또는 `-`만 있는 섹션 (보완 가능한 경우)

### 결과 보고 형식

```markdown
## [{YYYY-MM-DD}] lint
### 발견된 문제
- [모순] {페이지A} vs {페이지B}: {내용}
- [고아] {페이지}: inbound 링크 없음
- [미완성 링크] {페이지}에서 [[{기술명}]] 참조하지만 페이지 없음

### 권장 조치
- {조치 1}
- {조치 2}
```

Lint 결과도 `wiki/_log.md`에 append한다.

---

## 파일 조작 규칙

- `raw/` 폴더의 원본 파일은 **절대 삭제하지 않는다** — `processed/`로만 이동
- `wiki/_log.md`는 **append-only** — 기존 내용 수정·삭제 금지
- 파일 인코딩은 UTF-8 기준
- 마케팅 문구나 일반 설명은 기술로 분류하지 않는다
- 버전 정보가 있으면 반드시 기술명에 포함한다
- 파일 이동은 Bash 툴의 `mv` 명령으로 처리 (`Move-Item`보다 경로 호환성 우수)

## 운영 팁

### 대용량 파일 처리
- YouTube/웨비나 클립은 70,000+ 토큰을 초과할 수 있다 — Read 시 `limit`/`offset`으로 분할 독취
- 먼저 앞부분(`limit=100`)을 읽어 유형·카테고리 판별 후, 필요한 섹션만 추가 독취

### _log.md 삽입 순서
- 새 항목은 **파일 상단**에 삽입 (최신 항목이 위, 오래된 항목이 아래)
- 기존 첫 번째 `## [날짜]` 항목 바로 위에 새 항목 추가

### Ingest 전 stub 페이지 확인
- 신규 페이지 생성 전, `wiki/` 내 같은 이름의 stub(1줄 빈 파일)이 있는지 확인
- stub가 있으면 신규 생성 대신 Write로 전면 작성 (Lint 로그에서 빈 페이지 목록 참조)

## 오류 처리

| 상황 | 처리 방법 |
|---|---|
| 파일을 읽을 수 없음 | `raw/errors/`로 이동 후 `raw/errors/errors.log`에 기록 |
| 기술 판별 불가 (기술자료) | `wiki/tech/etc/`에 저장, 태그에 `#unclassified` 추가 |
| 유형 판별 불가 (보고서/가이드라인) | `wiki/work/reports/`에 저장, 태그에 `#unclassified` 추가 |
| 이미 처리된 파일 | 스킵 후 `_log.md`에 기록 |

---

## 직접 실행 명령어

```bash
# Ingest: 전체 raw/ 처리
claude "raw/ 폴더의 모든 파일을 Ingest 오퍼레이션으로 처리해줘"

# Ingest: 특정 파일만
claude "raw/파일명.md 를 Ingest 오퍼레이션으로 처리해줘"

# Query: 위키 기반 질문
claude "RAG와 GraphRAG의 차이점을 wiki/ 기반으로 설명하고 가치 있으면 위키에 파일링해줘"

# Lint: 위키 건강 체크
claude "wiki/ 폴더 전체를 Lint 오퍼레이션으로 점검해줘"

# 인덱스 재생성
claude "wiki/ 폴더를 스캔해서 _index.md를 최신화해줘"
```

---

## 메모

- 이 파일(CLAUDE.md)은 Claude Code가 `옵시디언/` 루트에서 실행될 때 자동으로 읽는 컨텍스트 파일입니다.
- Obsidian의 **Dataview 플러그인**을 활용하면 `_index.md`를 frontmatter 기반 동적 쿼리로 대체할 수 있습니다.
- **Graph View**로 위키 형태를 시각화하면 고아 페이지와 허브 페이지를 한눈에 파악할 수 있습니다.
- **Obsidian Web Clipper**로 웹 기사를 마크다운으로 변환해 `raw/`에 바로 저장할 수 있습니다.
- 정기 실행이 필요한 경우 Claude Code의 `/schedule` 기능 또는 OS cron과 연동하세요.
