---
layout: default
title: 부록
nav_order: 7
permalink: /appendix/
---

# 부록

## 시스템명 "Arda"의 의미

**Arda(아르다)** 는 톨킨의 요정어(퀘냐)로 **"영역(Realm)"** 을 뜻한다.
톨킨 세계관에서 요정·인간·난쟁이 등 모든 종족이 살아가는 터전의 이름이며,
보통명사로는 "경계가 정해진 장소, 하나의 영역"을 가리킨다.

이 이름을 시스템명으로 정한 이유는 두 가지다.

1. **"사람들이 모여 사는 터전"이라는 뜻이 채용과 곧바로 이어진다.**
   회사에 사람을 들이는 일은 곧 터전을 이루는 일이다.
2. **심판자가 아니다.** 후보였던 Argus(감시자)·Themis(정의의 여신)를 뺀
   이유는 "판정하는 존재라서 우리 원칙과 충돌한다"는 것이었다. Arda는
   판단하는 주체가 아니라 **판단이 일어나는 장소**다. 이는 시스템의
   정체성과 일치한다 — **도구는 자리를 마련하고, 판단은 그 안의 사람이
   한다.** AI 요약·파싱·에이전트는 심사를 돕는 무대를 깔 뿐, 합격·불합격을
   가르는 것은 언제나 채용 담당자다.

## 용어 정의

| 용어 | 정의 |
|------|------|
| ATS | Applicant Tracking System — 지원자 추적 관리 시스템 |
| Arda | 본 프로젝트의 시스템 코드명. 퀘냐로 "영역(Realm)" — 위 "시스템명 Arda의 의미" 참조 |
| 칸반 보드 | 지원자 카드를 단계별 열에 배치하여 채용 프로세스를 시각화하는 UI |
| RAG | Retrieval-Augmented Generation — 문서 검색 후 LLM이 답변을 생성하는 방식 |
| Tool-Calling Agent | 자연어 명령을 파싱하여 적절한 API 도구를 호출하는 AI 에이전트 |
| presigned URL | S3에서 발급하는 일회성 인증 URL. 브라우저에서 서버를 거치지 않고 직접 업로드 가능 |
| SES | AWS Simple Email Service — 이메일 발송 서비스 |
| SQS | AWS Simple Queue Service — 메시지 큐 서비스 (비동기 처리용) |
| pgvector | PostgreSQL 확장. 벡터 임베딩을 저장하고 유사도 검색 수행 |
| ADR | Architecture Decision Record — 아키텍처 의사결정 기록 문서 |
| RBAC | Role-Based Access Control — 역할 기반 접근 제어 |
