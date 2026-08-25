---
layout: default
title: 주요 개발 수행 지침
nav_order: 5
permalink: /guidelines/
---

# 주요 개발 수행 지침

## 일반 사항

### 개발 방법론

애자일 스크럼 방식으로 진행한다. 전체 10주를 5개 스프린트(각 2주)로 나누고, 스프린트마다 동작하는 결과물을 산출한다.

| 스프린트 | 기간 | 목표 |
|---------|------|------|
| Sprint 1 | 08/20 ~ 09/02 | 기반 구축 — DB 스키마, 프로젝트 뼈대, 개발 환경 |
| Sprint 2 | 09/03 ~ 09/16 | 핵심 CRUD — 공고·지원자 API, 검색, S3 업로드 |
| Sprint 3 | 09/17 ~ 09/30 | 칸반·전환·알림 — 칸반 UI, 단계 전환, 메일 발송 |
| Sprint 4 | 10/01 ~ 10/14 | AI 기능·고도화 — 이력서 파싱, RAG, Agent |
| Sprint 5 | 10/15 ~ 10/27 | 통합·안정화 — 통합 테스트, 성능 측정, 최종 배포 |

### 스프린트 운영 규칙

- **데일리 스크럼** — 매일 10분, 어제 한 일 / 오늘 할 일 / 블로커 공유
- **스프린트 리뷰** — 스프린트 종료일에 동작하는 결과물 시연
- **스프린트 회고** — 리뷰 직후 진행, 개선 사항 다음 스프린트에 반영

---

## 개발 표준 및 산출물

### 기술 스택

**BACKEND** — Python · FastAPI · PostgreSQL

**FRONTEND** — React · Vite · TypeScript

**INFRA** — Docker · AWS (EC2 · S3 · SES · SQS) · GitHub Actions · Vercel

**AI** — Claude API · pgvector · LangGraph

### 코드 관리 규칙

| 항목 | 기준 |
|------|------|
| 버전 관리 | Git (GitHub) |
| 브랜치 전략 | feature 브랜치 → PR → main 머지 |
| 코드 리뷰 | PR 머지 전 최소 1인 리뷰 필수 |
| 커밋 메시지 | Conventional Commits (`feat:`, `fix:`, `docs:`, `ci:`) |

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
| 단위 테스트 | API 엔드포인트, AI 모듈 | PyTest | PR 머지 전 |
| 단위 테스트 | React 컴포넌트 | Vitest | PR 머지 전 |
| 통합 테스트 | API ↔ DB 연동 | PyTest | 스프린트 종료 시 |
| E2E 테스트 | 주요 사용자 시나리오 | 수동 QA 체크리스트 | 스프린트 리뷰 |
| 성능 테스트 | 이력서 파싱, RAG 응답, 검색 | 더미 10만 건 기준 | Sprint 5 |
