---
layout: default
title: 이태우 — AI Agent Developer Portfolio
---

# 🧑‍💻 안녕하세요, Agent 개발자 이태우입니다.

## 👋 About Me

> 금융·보험 AX 프로젝트에서 LLM Agent의 실행 흐름, 상태 관리, 데이터 연계 문제를 직접 다루며 Agent 개발 경험을 쌓고 있습니다.

- Google ADK, MCP/Tool integration, session-state 관리
- 금융권 폐쇄망 환경에서 Qwen/vLLM 기반 Agent 개발
- 학부 연구생 시절 제1저자 논문 4편 게재 — [Publications](#-publications) 참고
- **Experience**: 2026.03 ~ 현재, Agent 개발
- **Contact**: 010-6609-0643 · [twl0812@naver.com](mailto:twl0812@naver.com) · [GitHub](https://github.com/twl0812)

---

## 🚀 Core Competencies

| 영역 | 내용 |
| --- | --- |
| Agent Workflow & State | Main/Sub Agent 실행 흐름, 업무 키 기반 상태 lifecycle, session/event 기반 실행 추적, explicit state propagation |
| Tool / System Integration | MCP, FunctionTool, 콜백, 연계 시스템(코어뱅킹/RPA/EAI) 결과를 반영하는 Agent 로직, JSON → DataFrame/Storage 변환 |
| Data-backed Agent Development | 원천 데이터 부족 시 데이터 담당자와 협의, 확보한 데이터로 mock dataset 직접 구성, 데이터 기반 business logic 구현 |
| Runtime Troubleshooting | SSE 연결 끊김, SSL/CA 설정, 모델 프로바이더 분기, 폐쇄망 Docker 운영 |

### 🔧 Tech Stack

| 구분 | 내용 |
| --- | --- |
| Agent | Google ADK, A2A Protocol, MCP, FunctionTool, Deterministic Router |
| RAG / Data | RAGFlow, Infinity, pandas, GCS, MySQL, Tibero |
| Backend / Web | Python, FastAPI, Streamlit, SSE(Server-Sent Events), pyodbc |
| Infra | Docker, MinIO, Redis/Valkey, 폐쇄망(Air-gapped) 환경 구축 |

*(임베디드 시스템 시절 스킬은 [Earlier Work](#-earlier-work) 참고)*

---

## 📂 AI Agent Developer Projects

### 🏦 Main Work — 상용 구축 프로젝트

#### 📌 기업 여신(Corporate Lending) AI 에이전트 `[실무 프로젝트 · 상용 구축 진행 중 · 팀 프로젝트]`

> 국내 시중은행 기업금융 여신 프로세스 대상 상용 구축 프로젝트 · 2026.06 ~ 진행 중

**Overview**

기업여신 업무 프로세스에 Agent를 투입하는 상용 구축 프로젝트입니다. 저는 OCR 이후 심사·검증·승인 업무 흐름에서 판단을 수행하는 **정책 Agent 개발**을 담당하고 있습니다. OCR·RPA·Bridge·EAI·코어뱅킹 연계는 각각 별도 담당 팀이 있으며, 현재 모든 시스템이 완전히 연계된 상태는 아닙니다.

**My Scope** — 팀 내 담당 파트: 기업여신 정책 Agent 개발 (RAG·백엔드·프론트엔드·대응(dialogue) 개발, 그리고 OCR·RPA·Bridge·EAI 연계는 별도 파트/팀 담당)

- 현업 요구사항 및 여신 업무 화면(33개) 분석 → Agent 개입 지점 정의
- OCR 문서 유형(약 25종) → 후속 심사·검증·승인 workflow 매핑
- 상담번호·승인신청번호 등 업무 키 기준 데이터·상태 연결 검토
- ADK event/state를 활용한 담당 Agent 실행 구간 구현

**Development under Integration Constraints — 시스템 연계 전 단계의 개발**

실제 개발 과정에서는 필요한 시스템 연계가 아직 열리지 않은 구간이 많았습니다. 이 문제를 아래 흐름으로 해결하며 진행했습니다.

1. 정책 Agent 판단에 필요한 데이터 항목 정의
2. 계정계/ADW 원천 데이터 중 부족한 항목은 데이터 담당자에게 직접 문의·제공 요청
3. 데이터 담당자가 Tibero에 제공한 데이터를 확보 — 금리 원장, 평가지표 등 관련 테이블을 최대 5개까지 직접 조인해 개발용 mock dataset 구성 (실제 원천 데이터 기반, 완전한 시스템 연계 전 단계)
4. 위 데이터를 근거로 Agent의 business logic(판단 로직) 구현 — 데이터 해석이 모호한 경우 현업 PL(비개발자, 업무 도메인 담당)에게 "이 상황에서 이 데이터라면 이 판단이 맞는지" 확인하며 로직을 검증

**Engineering Decision — 담당 구간 내 Deterministic Routing 적용**

- 일부 Agent 실행 구간에서 Qwen이 tool 호출·실행 순서를 직접 결정할 경우, 동일 입력에도 실행 경로가 달라지는 문제를 확인
- 해당 구간에서는 session/event 결과를 코드에서 파싱해 state를 갱신한 뒤 다음 실행을 결정하는 routing 방식으로 전환
- 적용 범위는 제가 담당한 정책 Agent 실행 구간에 한정

**Current Status** *(2026.08)*

- **Completed**: 담당 업무·화면·문서 기반 Agent 개입 지점 정의, mock dataset 기반 business logic 개발, session/event 기반 explicit state 구조 검증, 담당 구간 Deterministic Routing 적용
- **In Progress**: 실제 시스템 연계 확대에 따른 데이터 소스 전환(mock → 실 연계), 비동기·연속 실행 구현, 상용 적용을 위한 검증·고도화

**Tech**: Python · Google ADK · Qwen 3.x/vLLM · MCP · Session/Event/State · Tibero · Closed Network

---

### 🛠 Internal Engineering — 사내 표준화

#### 📌 사내 ADK/A2A 에이전트 개발 표준화 `[사내 표준화 작업 · 팀 프로젝트]`

> 팀에서 재사용할 수 있는 ADK 데이터 에이전트 플랫폼과 A2A 런타임(`agent_a2a`) 구조, 개발 가이드를 표준화 · 2026.03 ~ 2026.05

**Overview**: ADK 데이터 에이전트 플랫폼과 A2A 런타임(`agent_a2a`)을 팀이 재사용할 수 있는 internal agent platform으로 구조화한 작업입니다. 플랫폼 전체는 팀 단위로 진행했습니다.

**Runtime**: ADK Agent를 A2A 기반 FastAPI runtime으로 노출할 수 있도록 agent 등록, request handling, task/session backend, model provider abstraction을 공통 구조(`agent_a2a`)로 정리했습니다.

**Standardization**: config 기반 로깅·모델 생성, 인메모리/DB 백엔드 전환, 폐쇄망 오프라인 배포를 포함한 운영 재사용성을 갖췄습니다. 에이전트 성숙도(LV1~LV5)와 Skill 라이프사이클, trigger rate 평가 방법론을 담은 개발 가이드도 함께 정리했습니다.

**My Contribution** — 위 팀 결과물 중 제가 직접 기여한 부분:

- Provider 분기 및 SSL 설정 관련 런타임 이슈 수정
- MCP 연결 범위 `tool_filter` 적용
- A2A runtime 구조 일부 구현·검증
- 개발 가이드 작성 참여

**Tech**: Python · Google ADK · A2A Protocol · MCP · FastAPI · loguru

---

### 🧪 Selected POCs — 단기 컨셉 검증

> 아래 2건은 상용 프로젝트와 별개로, 짧은 기간(1~2개월) 동안 기술 검증 목적으로 진행한 POC입니다.

#### 📌 보험사 AI 에이전트 POC (광고 사전심의 · 데이터 분석) `[POC · 팀 프로젝트]`

> 보험 광고 사전심의 에이전트와, 마케팅 데이터 분석용 Text-to-SQL 함께 개발한 POC · 2026.05 ~ 2026.06

**Overview**: ADK Main/Sub Agent + RAGFlow 규정 검색으로 광고 사전심의를 수행하고, Streamlit 기반 POC Web(SSE 스트리밍·세션 관리·인증)으로 서비스. 병행하여 자연어 질의 → SQL 생성 → 검증 → 실행까지 이어지는 Text-to-SQL 에이전트를 개발

**트러블슈팅 — 출력 형식 불안정 (Text-to-SQL)**

- **문제**: LLM이 JSON 형식으로 응답해야 하는 구간에서 자꾸 자연어로 출력하는 문제 발생
- **1차 시도**: 프롬프팅으로 해결을 시도했으나 안정적으로 해결되지 않음
- **해결**: after callback 훅(ADK)으로 모델 출력을 후처리·보정하도록 변경

**트러블슈팅 — 컨텍스트/토큰 관리 (Text-to-SQL)**

- **문제**: vLLM `max_model_len=26000`/`max_output_tokens=4096` 제약 아래 SQL 실행 결과가 툴 응답·session state·모델 출력 echo 세 군데에 중복 적재
- **해결**: 응답을 slim/full로 분리하고 사용 후 state를 정리하는 방식으로 컨텍스트 축소. 코드 리뷰로 SQL 사전 검증 부재, 결과 데이터 영구 저장 누락 등의 갭도 함께 식별

**Tech**: Python · Google ADK · Streamlit · RAGFlow · MySQL · MinIO · SSE · vLLM · pandas

---

#### 📌 대형 건설사 건강분석 및 운동 추천 에이전트 `[POC · 팀 프로젝트]`

> 임직원의 신체·활동·수면 데이터를 분석해 맞춤 운동/수면 코스를 추천하는 RAGFlow 기반 워크플로우 에이전트 POC

**Workflow**: 체중·골격근량·체지방률·BMI, 걸음 수·활동/운동 시간, 수면 시간·점수·취침/기상 시각을 입력받아 분석 → 운동 추천 → 수면 추천 → 요약 통합 순으로 처리합니다. 운동·수면 결과와 사용자 프로필을 하나의 구조로 정규화하는 `get_summary_input` FunctionTool(Google ADK)이 최종 요약 입력을 구성합니다.

**트러블슈팅 — 수치 기반 판단 정확도**

- **문제**: LLM이 수치 데이터를 근거로 위험 여부를 판단하도록 했으나, 수치를 잘못 식별하는 문제가 발생
- **1차 시도**: Chain-of-Thought(CoT) 프롬프팅으로 개선을 시도했으나 문제가 계속 발생
- **해결**: 수치 비교 로직을 Python 코드로 분리 — 코드가 수치를 직접 비교해 "위험" 등 판정 결과를 결정론적으로 반환하도록 변경

**Tech**: Python · Google ADK · FunctionTool · RAGFlow

---

## 📖 Earlier Work

인천대학교 임베디드시스템공학 재학 중 진행한 프로젝트와 연구입니다
- **TROY OJ** — LLM 기반 C 언어 학습 지원 Online Judge (Capstone Design). LLM 기반 코드 피드백, 단계별 학습 모드, 태그 기반 문제 추천을 구현. Frontend/Backend(PHP, Python, MySQL) 담당. 🔗 [GitHub](https://github.com/comjke33/Capstone_Design_Troy)
- **Sleep Posture Correction Device** — 수면 자세 교정 웨어러블 디바이스 (Capstone Design). RNN 기반 수면 단계 판별(Light/Deep/REM), 가속도 센서 기반 자세 인식·진동 교정을 구현. PPG 전처리 및 RNN 모델, 센서 알고리즘 담당. 🔗 [GitHub](https://github.com/twl0812/inu-capstone-project-zzzk.github.io)
- **인천대학교 ICON 랩 학부연구생 (2023.01 ~ 2024.08)** — 센서 네트워크 기반 보안 감시 시스템 연구에 참여해 제1저자 논문 4편을 게재 (아래 Publications 참고)

**Skills(당시)**: C, C++ · Ubuntu(Linux) · MySQL · Python, TensorFlow, Keras, NumPy · Git

---

## 📄 Publications

- *Designing deep learning-enabled surveillance model with classified security levels for smart area networks* – Elsevier (2025)

    [🔗 Link](https://www.sciencedirect.com/science/article/abs/pii/S1570870525000125)

- *Reinforcing Deep Learning-Enabled Surveillance with Smart Sensors* – Sensors (2025)

    [🔗 Link](https://www.mdpi.com/1424-8220/25/11/3345)

- *Enhancing Smart Building Surveillance Systems in Thin Walls: An Efficient Barrier Design* – Sensors (2024)

    [🔗 Link](https://www.mdpi.com/1424-8220/24/2/595)

- *Maximum Activation 3D Cube Transition System for Virtual Emotion Surveillance* – IEEE Communications Letters (2023)

    [🔗 DOI: 10.1109/LCOMM.2023.3272671](https://doi.org/10.1109/LCOMM.2023.3272671)

---

## 📑 Education

🎓 **Incheon National University — Embedded Systems Engineering**

- Relevant Courses: Data Structures · Operating Systems · Database · AI · Computer Vision

---

## 🔒 Disclosure

고객사명, 내부 시스템 상세, 실제 규정·운영 데이터 및 소스코드는 제외했습니다. 진행 중인 프로젝트는 완료 성과 대신 실제 담당 범위와 해결한 엔지니어링 문제를 기준으로 기술했으며, 팀 프로젝트의 경우 개인 기여 범위를 구분해 작성했습니다.
