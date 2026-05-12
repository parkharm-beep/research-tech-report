---
tags: [technology, tech-note, infrastructure]
created: 2026-05-12
source_count: 0
status: stub
---

# 소프트웨어 정의 인프라 (SDx)

## 1. 개요 (Overview)
- **기술명:** SDx (Software-Defined Everything, 소프트웨어 정의 인프라)
- **정의:** 하드웨어의 기능과 제어 로직을 소프트웨어 계층으로 추상화하여, API·자동화·프로그래밍으로 인프라를 관리하는 패러다임
- **배경:** 물리적 하드웨어에 종속된 전통적 인프라 관리의 한계를 극복하기 위해, 클라우드·가상화 기술과 함께 SDN(네트워크), SDS(스토리지), SDDC(데이터센터) 등이 발전했다.

## 2. 주요 특징 (Key Features)
- **제어 평면 분리:** 하드웨어의 데이터 전달(Data Plane)과 제어 로직(Control Plane)을 분리
- **API 기반 관리:** 하드웨어를 API로 프로그래밍 → 자동화·IaC(Infrastructure as Code) 가능
- **주요 하위 분야:**
  - **SDN (Software-Defined Networking):** 네트워크 스위치·라우터를 소프트웨어로 제어
  - **SDS (Software-Defined Storage):** 이기종 스토리지를 단일 풀로 통합·관리
  - **SDDC (Software-Defined Data Center):** 컴퓨팅·네트워크·스토리지 전체를 소프트웨어 정의

## 3. 작동 원리 (How it Works)
- 하이퍼바이저·컨테이너([[Docker]])가 컴퓨팅 자원을 가상화
- SDN 컨트롤러가 네트워크 플로우를 중앙에서 프로그래밍
- Terraform·Ansible 등 IaC 도구로 선언적 인프라 정의·배포

## 4. 주요 활용 사례 (Use Cases)
- 사례 1: 클라우드 인프라 자동화 (AWS VPC, GCP VPC 등)
- 사례 2: Kubernetes를 통한 컨테이너 오케스트레이션 — SDx의 컴퓨팅 영역 적용
- 사례 3: AI 데이터센터 구축 — [[모듈러 AI 데이터센터]]가 SDx 원칙 적용

## 5. 장단점 (Pros & Cons)
### 장점 (Pros)
- 인프라 프로비저닝 자동화 — 수주 → 수분으로 단축
- 하드웨어 벤더 종속 완화
### 단점 (Cons)
- 소프트웨어 레이어 추가로 복잡도 증가
- 하드웨어 성능의 최대치 활용에 오버헤드 발생 가능

## 6. 관련 기술 (Related Technologies)
- [[Docker]]
- [[모듈러 AI 데이터센터]]
- [[CUDA]]

## 7. 참고 자료 (References)
- VMware SDx 공식 문서
- OpenFlow 프로토콜 스펙
