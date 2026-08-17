---
layout: default
title: 이태우 — AI Agent Developer Portfolio
---

# 🧑‍💻 안녕하세요. 연구와 개발 모두 가능한 Agent 개발자 이태우입니다.

## 👋 About Me

- **Name**: 이태우
- **Major**: Embedded Systems Engineering
- **Email**: [twl0812@naver.com](mailto:twl0812@naver.com)
- **GitHub**: [github.com/twl0812](https://github.com/twl0812)
- 모델의 추론(reasoning)과 코드의 결정론적 제어(deterministic control)를 분리해, 복잡한 엔터프라이즈 프로세스를 추적 가능하고 재현 가능한 Agent workflow로 만드는 것이 강점입니다.
- 은행·보험 등 엔터프라이즈 환경에서 Agent workflow, tool/MCP integration, session/state 관리, 폐쇄망 인프라를 직접 다뤄왔습니다.

> **Positioning**: 제 강점은 파운데이션 모델을 직접 학습시키는 것이 아니라, LLM·툴·엔터프라이즈 규칙·레거시 시스템을 하나의 실행 가능한 워크플로로 엮어서, 그 동작을 관찰하고 통제할 수 있게 만드는 것입니다.

---

## 🚀 Core Competencies

| 영역 | 내용 |
| --- | --- |
| Agent Architecture | Main/Sub Agent 위임 구조, 라우터, 순차 실행, 도메인 경계 설계 |
| Workflow Orchestration | 비동기/연속 실행, 업무 키 기반 상태 lifecycle, tool·RPA·legacy 연계 흐름 설계 |
| Tool & System Integration | MCP, FunctionTool, 콜백, 코어뱅킹/BPR/RPA/EAI 연동, JSON → DataFrame/Storage 변환 |
| State & Runtime Engineering | session/event 기반 실행 추적, explicit state propagation, A2A/ADK 런타임 구조화, rerun에도 안전한 웹 세션 |
| Operational Troubleshooting | SSE 연결 끊김, SSL/CA 설정, 모델 프로바이더 분기, 폐쇄망 Docker 운영 |

### 🔧 Tech Stack

| 구분 | 내용 |
| --- | --- |
| Agent | Google ADK, A2A Protocol, MCP, FunctionTool, Deterministic Router |
| LLM | Gemini 2.5 Flash, GPT-4o-mini, Qwen 3.x / vLLM (온프레미스) |
| RAG / Data | RAGFlow, Infinity, pandas, GCS, MySQL |
| Backend / Web | Python, FastAPI, Streamlit, SSE(Server-Sent Events), pyodbc |
| Infra | Docker, MinIO, Redis/Valkey, 폐쇄망(Air-gapped) 환경 구축 |
| Research | 비지도 학습, 이상탐지, 실험 설계 및 평가지표 |

*(임베디드 시스템 시절 스킬은 [Earlier Work](#-earlier-work) 참고)*

---

## 📂 AI Agent Developer Projects

### 📌 Project 01 — 기업 여신(Corporate Lending) AI 에이전트 `[실무 프로젝트 · 상용 구축 진행 중]`

> 국내 시중은행 기업금융 여신 프로세스 대상 상용 구축 프로젝트 · 2026.06 ~ 진행 중 (02~03은 POC, 04는 사내 표준화)

**Overview**

기업여신 업무 프로세스에 Agent를 투입하는 상용 구축 프로젝트입니다. OCR 이후 심사·검증·승인 업무 흐름에 Agent가 개입하며, 코어뱅킹·BPR·RPA·EAI·브릿지 앱을 가로지르는 업무 키 기반 workflow로 설계하고 있습니다.

**My Scope**

- 여신 업무 33개 화면 분석 → Agent 개입 지점 정의
- OCR 문서 유형(약 25종) → 후속 심사·검증·승인 workflow 매핑
- 상담/승인 ID 등 업무 키 기준 상태 연계 구조 설계
- OCR 엔진은 스코프 밖, OCR 이후 Agent execution flow 담당

**Engineering Decision — Deterministic Control**

초기에는 MCP tool call·JSON schema·실행 순서까지 Qwen이 직접 판단하도록 설계했으나, 동일 입력에도 실행 경로가 안정적으로 재현되지 않는 경우가 나타났습니다. 금융 업무 특성상 실행 경로와 상태 전이의 예측 가능성·통제 가능성이 중요하다고 판단해, orchestration control을 LLM에서 코드 레벨 router로 이동했습니다. 이후 session/event 조회 → JSON parsing → state update → downstream Agent 실행 구조로 재구성해, reasoning과 execution control을 분리하고 추적성·재현성을 확보했습니다.

**Architecture — Enterprise Workflow Orchestration**

Agent 실행은 답변 생성에서 끝나지 않고, tool/MCP/API/RPA 호출 → 상태 변경 → 후속 Agent 또는 legacy 시스템 반환으로 이어지는 구조입니다. 업무 키(상담/승인 ID) 기준 상태 lifecycle 관리, 비동기 처리 결과 추적, 이전 결과를 다음 단계 입력으로 잇는 연속 실행, RPA 검증 결과 반영을 하나의 workflow로 연결하고 있습니다.

**Current Status** *(2026.08)*

- **Completed**: 주요 업무·화면·문서 기반 Agent 개입 지점 및 workflow pattern 설계, MCP 연계·session/event 기반 explicit state 구조 검증, Deterministic Router 설계·구현
- **In Progress**: 비동기·연속·RPA 기반 workflow 구현, 기존 시스템 연계 확장, 상용 적용을 위한 검증·고도화

**Tech**: Python · Google ADK · Qwen 3.x/vLLM · MCP · Session/Event/State · Enterprise API · Closed Network

---

### 📌 Project 02 — 보험사 AI 에이전트 POC (광고 사전심의 · 데이터 분석) `[POC]`

> 보험 광고 사전심의 에이전트와, 마케팅 데이터 분석용 Text-to-SQL 에이전트(UC1~UC3)를 함께 개발한 POC · 2026.05 ~ 2026.06

**Overview**: ADK Main/Sub Agent + RAGFlow 규정 검색으로 광고 사전심의를 수행하고, Streamlit 기반 POC Web(SSE 스트리밍·세션 관리·인증)으로 서비스. 병행하여 자연어 질의 → SQL 생성 → 검증 → 실행까지 이어지는 Text-to-SQL 에이전트를 개발

**트러블슈팅 — 데이터/세션 안정성**

- **문제**: RAGFlow/MinIO 볼륨이 Docker Compose 설정 실수로 RAM(휘발성) 경로에 저장돼 데이터가 유실됨. Streamlit의 rerun 특성 때문에 세션이 끊기는 문제도 발생
- **해결**: 볼륨 마운트 수정으로 영구 저장소 확보, `session_state` 명시적 관리로 세션 유지. SSE 연결 끊김은 아키텍처 경계 문제로 보고 재연결/에러 핸들링을 별도 설계

**트러블슈팅 — 컨텍스트/토큰 관리 (Text-to-SQL)**

- **문제**: vLLM `max_model_len=26000`/`max_output_tokens=4096` 제약 아래 SQL 실행 결과가 툴 응답·session state·모델 출력 echo 세 군데에 중복 적재
- **해결**: 응답을 slim/full로 분리하고 사용 후 state를 정리하는 방식으로 컨텍스트 축소. 코드 리뷰로 SQL 사전 검증 부재, 결과 데이터 영구 저장 누락(MinIO/S3 미연동) 등의 갭도 함께 식별

**기타**: DB 접근 라이브러리를 `ibm_db`에서 `pyodbc`로 전환

**Tech**: Python · Google ADK · Streamlit · RAGFlow · MySQL · MinIO · SSE · vLLM · pandas

---

### 📌 Project 03 — 대형 건설사 건강분석 및 운동 추천 에이전트 `[POC]`

> 임직원의 신체·활동·수면 데이터를 분석해 맞춤 운동/수면 코스를 추천하는 RAGFlow 기반 워크플로우 에이전트 POC

**Workflow**: 체중·골격근량·체지방률·BMI, 걸음 수·활동/운동 시간, 수면 시간·점수·취침/기상 시각을 입력받아 분석 → 운동 추천 → 수면 추천 → 요약 통합 순으로 처리합니다. 운동·수면 결과와 사용자 프로필을 하나의 구조로 정규화하는 `get_summary_input` FunctionTool(Google ADK)이 최종 요약 입력을 구성합니다.

**Tech**: Python · Google ADK · FunctionTool · RAGFlow

---

### 📌 Project 04 — 사내 ADK/A2A 에이전트 개발 표준화 `[사내 표준화 작업]`

> 팀에서 재사용할 수 있는 ADK 데이터 에이전트 플랫폼과 A2A 런타임(`agent_a2a`) 구조, 개발 가이드를 표준화 · 2026.03 ~ 2026.05

**Overview**: ADK 데이터 에이전트 플랫폼과 A2A 런타임(`agent_a2a`)을 팀이 재사용할 수 있는 internal agent platform으로 구조화한 작업입니다.

**Runtime**: ADK Agent를 A2A 기반 FastAPI runtime으로 노출할 수 있도록 agent 등록, request handling, task/session backend, model provider abstraction을 공통 구조(`agent_a2a`)로 정리했습니다. MCP 연결 범위는 `tool_filter`로 제한했고, provider 분기 및 SSL 설정 관련 런타임 이슈를 수정했습니다.

**Standardization**: config 기반 로깅·모델 생성, 인메모리/DB 백엔드 전환, 폐쇄망 오프라인 배포를 포함한 운영 재사용성을 갖췄습니다. 에이전트 성숙도(LV1~LV5)와 Skill 라이프사이클, trigger rate 평가 방법론을 담은 개발 가이드도 함께 정리했습니다.

**Tech**: Python · Google ADK · A2A Protocol · MCP · FastAPI · loguru

---

## 📖 Earlier Work

인천대학교 임베디드시스템공학 재학 중 진행한 프로젝트와 연구입니다.

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

실제 고객사명, 내부 시스템 상세, 실제 규정 내용, 소스코드, 운영 데이터는 이 포트폴리오에서 제외했습니다. 검증되지 않은 성과 수치는 기재하지 않았으며, 현재 진행 중인 은행 프로젝트는 완료 지표가 아니라 실제로 해결한 엔지니어링 문제와 담당 범위를 기준으로 기술했습니다.
