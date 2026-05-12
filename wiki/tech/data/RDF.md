---
tags: [technology, tech-note, semantic-web, rdf]
created: 2026-04-23
source_count: 1
status: draft
source: [[raw/processed/AI에이전트! 엑셀로 일시키면 40점, 온톨로지로 시키면 90점 (이경일 솔트룩스 대표).md]]
---

# RDF (Resource Description Framework)

## 1. 개요 (Overview)
- **기술명:** RDF (Resource Description Framework)
- **정의:** 웹상의 자원(Resource)에 대한 정보를 표현하기 위해 W3C에서 제정한 표준 모델로, 데이터를 <주어, 술어, 목적어> 형태의 삼중항(Triple) 구조로 기술하는 그래프 기반 데이터 모델입니다.
- **배경:** 서로 다른 메타데이터 형식 간의 호환성을 확보하고, 기계가 웹상의 정보를 의미적으로 이해하고 처리할 수 있는 '시맨틱 웹'을 구현하기 위해 등장했습니다.

## 2. 주요 특징 (Key Features)
- **삼중항(Triple) 모델:** 모든 정보를 '주어(Subject)-술어(Predicate)-목적어(Object)'의 세 가지 요소로 분해하여 표현합니다.
- **URI 기반 식별:** 자원과 속성을 전역적으로 고유한 URI(Uniform Resource Identifier)로 식별하여 데이터 간의 연결성을 보장합니다.
- **스키마 유연성:** 정해진 구조 없이 새로운 데이터나 관계를 자유롭게 추가할 수 있는 그래프 구조를 가집니다.
- **상호운용성:** 다양한 플랫폼과 도메인 간의 데이터를 통합하고 교환하는 데 최적화되어 있습니다.

## 3. 작동 원리 (How it Works)
- **RDF 그래프:** 여러 개의 삼중항이 모여 하나의 거대한 방향성 그래프(Directed Graph)를 형성합니다.
- **직렬화 형식 (Serialization):** RDF 모델을 파일 형태로 저장하거나 전송하기 위한 다양한 형식이 존재합니다.
    - **RDF/XML:** XML 기반의 초기 표준 형식.
    - **Turtle (Terse RDF Triple Language):** 인간이 읽기 쉬운 텍스트 기반 형식.
    - **JSON-LD:** JSON을 확장하여 링크드 데이터(Linked Data)를 표현하는 형식 (가장 널리 쓰임).
    - **N-Triples:** 한 줄에 하나의 삼중항을 기술하는 단순한 형식.

## 4. 주요 활용 사례 (Use Cases)
- **시맨틱 웹 및 링크드 데이터(Linked Data):** 웹상의 파편화된 데이터를 의미적으로 연결하여 거대한 지식망을 구축합니다.
- **지식그래프 구축:** 구글, 위키데이터(Wikidata) 등에서 개체 간의 관계를 정의하는 기본 모델로 사용됩니다.
- **메타데이터 관리:** 도서관 서지 정보, 디지털 자산의 저작권 및 속성 정보를 표준화된 방식으로 기술합니다.
- **데이터 통합:** 서로 다른 데이터베이스(RDB, NoSQL 등)에 흩어진 정보를 하나의 공통 모델로 통합 분석합니다.

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 데이터 모델이 매우 유연하여 복잡하고 변화가 잦은 정보 표현에 유리합니다.
- 전 세계적으로 고유한 URI를 사용하여 데이터 충돌 없이 전역적인 데이터 연결이 가능합니다.
- 기계가 데이터의 의미를 해석할 수 있어 자동화된 추론과 처리가 가능합니다.
### 단점 (Cons)
- 관계형 데이터베이스(RDB)에 비해 데이터 구조의 복잡도가 높고 학습 곡선이 있습니다.
- 대규모 데이터셋에 대한 쿼리(SPARQL) 성능이 전통적인 SQL보다 느릴 수 있습니다.
- XML 기반 직렬화의 경우 가독성이 떨어지고 데이터 크기가 커지는 경향이 있습니다.

## 6. 관련 기술 (Related Technologies)
- [[온톨로지]]
- [[지식그래프]]
- OWL (Web Ontology Language)
- SPARQL (RDF 전용 쿼리 언어)
- JSON-LD

## 7. 참고 자료 (References)
- [W3C RDF 1.1 Primer](https://www.w3.org/TR/rdf11-primer/)
- [Wikipedia: Resource Description Framework](https://en.wikipedia.org/wiki/Resource_Description_Framework)
- [[raw/processed/AI에이전트! 엑셀로 일시키면 40점, 온톨로지로 시키면 90점 (이경일 솔트룩스 대표).md]]

---

> [!note] 2026-05-07 소스 추가: AI 에이전트 지식 표현 언어로서의 RDF/OWL
> RDF와 OWL은 온톨로지를 표현하는 국제 표준 언어로 W3C 및 ISO에서 표준화되어 있다. AI 에이전트 간 지식 공유([[시맨틱 패브릭]])를 위한 공통 언어로 기능하며, 특히 OWL은 추론(Reasoning)을 중심으로 다루는 온톨로지 언어다. 의료 표준(SNOMED), 국방 체계(JDC2) 등에서 이미 실전 활용 중.
