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

## 핵심 기능 5축 (2026-09-03 기준)

| # | 축 | 내용 | 상태 |
|---|-----|------|------|
| 1 | 채용 파이프라인 통합 관리 | 공고 등록과 공개 지원 링크 → 접수(이력서는 브라우저에서 저장소로 직행) → 테이블/칸반에서 심사·드래그·일괄 단계 변경 → 모든 이동 이력 기록 → 안내 메일 자동 발송 → 평가·면접관 배정 → 합불 | ✅ 완료 |
| 2 | 면접 일정 자동화 | 면접관 가용 시간 → 후보 슬롯 → 지원자 공개 링크 선택 → 즉시 확정·양측 통보 → 캘린더 | ✅ 완료 |
| 3 | AI 에이전트 "아르" | 접수 즉시 지원자 요약(요지·적합·우려 3단계) 생성 · 자연어 한 문장으로 검색·단계 변경(변경 작업은 반드시 사람이 확인 카드로 승인) · 의미 기반 검색(RAG) · AI 면접(텍스트) · 성향 설문 관찰 요약 · AI 사용 비용을 호출마다 기록 | ✅ 완료 (음성 인식은 W4) |
| 4 | 멀티 클라이언트 | React 웹 + Flutter 앱이 같은 FastAPI 사용 (웹 15 · 앱 16 화면) | 🔨 앱 연동 진행 |
| 5 | 운영 체계 | Docker · AWS 실배포 · alembic · 구조화 로깅 · CI · 계약 문서 동기화 | ✅ 완료 |

**규모(09/03)**: API 68개 · pytest 638건 · ADR 24건 · 커밋 352 · 필수 기능 27/27 구현
{: .fs-3 }
