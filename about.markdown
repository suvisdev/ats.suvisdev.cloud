---
layout: default
title: 프로젝트 소개
nav_order: 3
permalink: /about/
---

# 프로젝트 소개

## Arda

채용 지원자 관리 시스템(ATS) — 공고 등록부터 지원 접수, 단계별 심사·평가, 합불 통보까지

---

## 주요 기능

| 기능 | 설명 |
|------|------|
| 지원자 칸반 보드 | 카드를 드래그해 단계 이동(지원 접수 → 서류 검토 → 면접 → 최종 합격/불합격). 모든 이동은 단계 이력으로 기록 |
| 단계 변경 자동 메일 | 단계 이동 시 지원자에게 메일 자동 발송. SQS 큐 + 워커로 비동기 처리, 실패 시 재시도 |
| 공고 관리 · 공개 지원 링크 | 채용 공고 등록·관리. 지원자는 로그인 없이 외부 공개 링크로 지원서 제출 |
| 이력서 S3 업로드 | presigned URL로 브라우저에서 S3에 직접 업로드 — 파일이 API 서버를 거치지 않는다 |
| 지원자 검색·필터 | 이름·학교·기술스택 검색과 필터. 더미 데이터 10만 건 기준으로 인덱스 튜닝 |
| 평가 · 면접관 배정 | 면접관 배정, 단계별 점수·코멘트 평가 기록 |
| 인증 · 권한 | JWT 인증, 역할 3종(관리자 / 채용담당자 / 면접관) |

---

## 기술 스택

**BACKEND** — Python · FastAPI · PostgreSQL

**FRONTEND** — React · Vite · TypeScript

**INFRA** — Docker · AWS (EC2 · S3 · SES · SQS) · GitHub Actions · Vercel

**AI** — Claude API · pgvector · LangGraph

---

## 아키텍처

| 계층 | 기술 | 배포 |
|------|------|------|
| Frontend | React + TypeScript + Vite | Vercel |
| Backend API | FastAPI (Python) | EC2 · Docker |
| Database | PostgreSQL + pgvector | Aurora Serverless v2 |
| 파일 저장 | S3 (presigned URL 업로드, SSE 암호화) | AWS S3 |
| 메일 발송 | SES + SQS (비동기 큐) | AWS SES/SQS |
| CI/CD | GitHub Actions | 자동 배포 |
