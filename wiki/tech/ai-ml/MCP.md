---
tags: [technology, tech-note, ai-ml]
created: 2026-05-12
source_count: 1
status: draft
source: [[raw/processed/비개발자를 위한 AI 바이브코딩, 하네스엔지니어링 웨비나 1  일정, 선수지식, 출발을 위한 준비.md]]
---

# MCP (Model Context Protocol)

## 1. 개요 (Overview)
- **기술명:** MCP (Model Context Protocol)
- **정의:** AI 모델(Claude 등)이 외부 툴·서비스와 표준화된 방식으로 연결되어 해당 툴을 직접 조작하거나 데이터를 주고받을 수 있게 하는 프로토콜
- **배경:** LLM이 단순 텍스트 생성을 넘어 실제 소프트웨어 도구를 제어하는 에이전트 역할을 수행하려면, AI와 외부 시스템 간의 표준 인터페이스가 필요해졌다. Anthropic이 제안하여 Claude Code 등에서 광범위하게 사용된다.

## 2. 주요 특징 (Key Features)
- **표준 연결 방식:** 각 툴마다 별도 통합 코드 없이 MCP 서버 하나로 AI와 연결
- **양방향 제어:** AI가 외부 툴의 데이터를 읽는 것뿐 아니라 직접 조작(쓰기·클릭·그리기) 가능
- **플러그인 구조:** 사용하려는 서비스(Figma, GitHub, 브라우저 등)의 MCP 서버를 설치·활성화
- **에이전트 루프 통합:** [[에이전틱 AI]]의 도구 호출(Tool Calling) 레이어로 동작

## 3. 작동 원리 (How it Works)
1. 사용자가 Claude Code 등에서 MCP 서버 활성화 (예: Figma MCP)
2. AI가 사용자 요청을 분석 후 MCP를 통해 Figma API를 호출
3. Figma 캔버스에 직접 도형·레이아웃·텍스트 생성
4. 결과를 AI가 확인 후 추가 수정 반복

예시 (웨비나에서 시연):
```
사용자: "트렌디하고 섹시하게 SNS 메인 화면이랑 로그인 화면 만들어줘"
→ Claude가 Figma MCP를 통해 직접 Figma에 UI 디자인 생성
```

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: **Claude + Figma MCP** — AI가 Figma에 자동으로 UI 디자인 생성
- 사례 2: **Claude Code + GitHub MCP** — AI가 직접 PR 생성, 브랜치 관리
- 사례 3: **Claude + 브라우저 MCP** — 웹 스크래핑, 폼 자동 입력
- 사례 4: **Claude + 데이터베이스 MCP** — 자연어 쿼리를 DB에 직접 실행

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- AI와 외부 서비스 통합의 표준화 → 생태계 확장 용이
- 비개발자도 AI에게 툴을 조작시키는 방식으로 복잡한 작업 처리 가능
- 에이전트 역량을 코드 작성 외 영역까지 확장
### 단점 (Cons)
- 각 서비스마다 별도 MCP 서버 설치 필요
- 연결 오류(disconnect, 버전 불일치 등) 발생 시 비개발자가 대처 어려움
- AI가 툴을 잘못 조작했을 때 되돌리기 어려운 경우 있음

## 6. 관련 기술 (Related Technologies)
- [[에이전틱 AI]]
- [[바이브코딩]]
- [[하네스엔지니어링]]
- [[프롬프트엔지니어링]]

## 7. 참고 자료 (References)
- [[raw/processed/비개발자를 위한 AI 바이브코딩, 하네스엔지니어링 웨비나 1  일정, 선수지식, 출발을 위한 준비.md]]
- https://www.youtube.com/watch?v=t67mA2_D5FA
