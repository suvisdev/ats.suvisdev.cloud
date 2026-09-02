---
layout: home
title: Home
nav_order: 0
permalink: /
---

# Eval-ATS

**AI 기반 채용 프로세스 자동화 및 지원자 통합 관리 플랫폼**
{: .fs-6 .fw-300 }

이력서 AI 파싱, 칸반 보드, Tool-Calling Agent, RAG 질의응답까지 — 채용 프로세스를 하나의 플랫폼에서 자동화합니다.
{: .fs-5 .fw-300 }

[목차 보기](/toc/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[GitHub](https://github.com/suvisdev/ats.suvisdev.cloud){: .btn .fs-5 .mb-4 .mb-md-0 }

---

<table class="cv-table">
<tr><td class="cv-label">사업명</td><td><div class="cv-main">AI 기반 채용 프로세스 자동화 및 지원자 통합 관리 플랫폼</div></td></tr>
<tr><td class="cv-label">시스템명</td><td><div class="cv-main">Arda (Eval-ATS)</div></td></tr>
<tr><td class="cv-label">개발 기간</td><td><div class="cv-main">2026년 8월 20일 (목) ~ 2026년 10월 27일 (화)</div><div class="cv-sub">총 69일 · 10주, 애자일 스크럼 (2주 1스프린트 · 총 5스프린트)</div></td></tr>
<tr><td class="cv-label">개발팀 : SEUK</td><td><div class="cv-main">진수택 · 이재우 · 이우정 · 김민아 · 박소연</div><div class="cv-sub">5명</div></td></tr>
<tr><td class="cv-label">문서 작성일</td><td><div class="cv-main">2026년 8월 20일</div></td></tr>
<tr><td class="cv-label">깃허브 주소</td><td><div class="cv-main"><a href="https://github.com/Team-Seuk/Arda">github.com/Team-Seuk/Arda</a></div><div class="cv-sub">문서 저장소 — <a href="https://github.com/suvisdev/ats.suvisdev.cloud">github.com/suvisdev/ats.suvisdev.cloud</a></div></td></tr>
<tr><td class="cv-label">문서 사이트</td><td><div class="cv-main"><a href="https://ats.suvisdev.cloud">ats.suvisdev.cloud</a></div></td></tr>
<tr><td class="cv-label">데모 사이트</td><td><div class="cv-main"><a href="https://arda.seuk.cloud">arda.seuk.cloud</a></div><div class="cv-sub">Vercel 배포 · API api.arda.seuk.cloud</div></td></tr>
</table>

---

## 주요 기능

| 기능 | 설명 |
|------|------|
| 지원자 칸반 보드 | 카드 드래그로 단계 이동, 모든 이동 이력 기록 |
| 단계 변경 자동 메일 | SQS 큐 + SES 비동기 발송, 실패 시 재시도 |
| AI 이력서 파싱 | PDF/DOCX → 이름·학력·경력·기술스택 자동 추출 |
| Tool-Calling Agent | 자연어 명령으로 지원자 검색·통계 처리 |
| RAG 질의응답 | 이력서·공고 임베딩 기반 자연어 답변 (pgvector) |
| 공개 지원 링크 | 로그인 없이 외부 링크로 지원서 제출 |
| 인증·권한 | JWT + RBAC 3종 (관리자/채용담당자/면접관) |
