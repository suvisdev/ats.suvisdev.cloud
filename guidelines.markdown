---
layout: default
title: 주요 개발 수행 지침
nav_order: 5
permalink: /guidelines/
---

# 주요 개발 수행 지침

## 일반 사항

### 개발 방법론

**도메인 오너제**(ADR-0007) — 한 사람이 도메인 하나(폴더)를 소유하고, 자기 도메인 로드맵이 작업의 유일한 기준이다. 도메인 안의 판단은 묻지 않고, 팀 채널에 올리는 것은 도메인 밖뿐이다(범위·마일스톤 변경, 다른 도메인이 기다리는 의존, 30분 넘게 막힌 문제).

**계약 우선 · UI가 스펙** — 1주차 목업 HTML이 스펙이고, ERD·API 문서가 전원의 계약이다. 변경은 코드·alembic 리비전과 같은 커밋으로만 허용한다. 프론트·앱은 계약 문서로 병렬 개발하고, 목데이터 폴백으로 서버 없이도 전 화면이 뜬다.

**ADR 기반 의사결정** — 기술·범위·윤리 결정 24건을 ADR로 남겼다. "안 한 것"에도 ADR이 있다(K8s·표정분석·음성분석 제외). 개정은 원문을 지우지 않고 절을 덧붙인다.

주차별 실행 계획은 [개발 일정](/schedule/) 참조.

### 협업 운영 규칙

- **push 전 검증 필수** — 자기 파트 실행·테스트 결과를 커밋 메시지 한 줄에 남긴다
- **테스트를 통과시키려고 우회·주석 처리 금지** — 실패하면 실패한 대로 기록한다
- **깨진 main은 묻지 않고 즉시 고친다** — 앞으로 고쳐 나가거나(fix-forward) 문제 커밋을 되돌린다(revert). 자동 검사(CI) 빨간불은 다른 작업보다 우선
- **되돌릴 수 있는 일은 그냥 한다** — 실험·프로토타입은 묻지 않고 결과만 공유. 물어야 할 것은 되돌리기 어려운 것뿐(스키마, 외부 서비스 계약, 마일스톤)
- **문서 = 코드와 같은 리듬** — 문서 갱신 없는 스키마·API 변경 금지, 배포·장애·이행은 재현 절차까지 기록

---

## 개발 표준 및 산출물

### 기술 스택

**BACKEND** — Python · FastAPI · SQLAlchemy · PostgreSQL 16 + pgvector · alembic

**FRONTEND** — React · Vite · TypeScript / **APP** — Flutter

**INFRA** — Docker Compose · AWS (EC2 · S3 · SES · SQS) · Caddy · GitHub Actions · Vercel

**AI** — Claude API(claude-haiku-4-5) 기본 + Ollama(qwen3:4b) 온프레미스 옵션(환경변수 스위치) · ko-sroberta 임베딩(로컬) · Whisper/faster-whisper STT

### 코드 관리 규칙

| 항목 | 기준 |
|------|------|
| 버전 관리 | Git (GitHub) |
| 브랜치 전략 | `pull --rebase` → 실행·테스트 → **main 직접 push** + 사후 공지(스키마·API·공용 문서·남의 폴더) |
| 코드 리뷰 | 리뷰 게이트 없음(08/28 개정) — CI가 안전망, 이의는 사후 revert가 기본값 |
| 커밋 메시지 | `type(기능번호): 한국어 요약` (Conventional Commits 변형) |

### 주요 산출물

| 구분 | 산출물 | 비고 |
|------|--------|------|
| 설계 | 시스템 아키텍처 다이어그램 | architecture.svg |
| 설계 | ERD (테이블 정의서) | erd.png |
| 개발 | 소스 코드 (Frontend + Backend + AI) | GitHub 저장소 |
| 개발 | API 명세 (Swagger) | FastAPI `/docs` 자동 생성 |
| 문서 | 개발 진행 보고서 | 본 Jekyll 사이트 |
| 인프라 | Docker 컨테이너 구성 | Dockerfile, docker-compose |
| 인프라 | CI/CD 파이프라인 | GitHub Actions |

---

## 품질 관리 및 테스트

### 품질 요구 사항

| 요구 ID | 항목 | 검수 기준 |
|---------|------|----------|
| PER-001 | **성능** — 이력서 파싱 E2E 3초 이내, RAG 응답 First Token 2초 이내 | 부하 테스트 시 목표 달성 |
| QUA-001 | **테스트** — 요구사항 ID별 PyTest 및 Vitest 자동화 단위 테스트 | 핵심 로직 코드 커버리지 80% 이상 |
| QUA-002 | **문서화** — API 엔드포인트 자동 문서화 (FastAPI Swagger) | /docs 접속 시 전체 API 명세 조회 가능 |

### 테스트 전략

| 단계 | 대상 | 도구 | 시점 |
|------|------|------|------|
| 백엔드 단위·API | 실제 PostgreSQL+pgvector, alembic 이행 후 실행 | pytest **638건** | push·PR마다 (CI) |
| 에이전트 회귀 하네스 | 실서버 /agent/chat에 시나리오 40개(기본 10+변형 30) — 정답·속도·창작 여부 기록 | 자체 하네스 | 모델·프롬프트 변경 시 |
| 프론트 | ESLint + tsc -b + vite build | CI | push·PR마다 |
| 앱 | Flutter 위젯 테스트 25파일 | flutter test | 상시 |
| 계약 대조 | 배포본 OpenAPI ↔ main 경로·스키마·인증 대조 | check_public_contract.py | 배포 후 (08/31 실측 35/35·62/62 일치) |
| E2E·QA | 필수 27기능 시나리오(전제→절차→기대) | qa-scenarios 문서 | 게이트 판정 |
| 성능 | 더미 10만 건 검색 튜닝 — 111ms → 7.8ms 실측 | perf-search 보고 | W4 재측정 |
