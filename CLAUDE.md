# Project: ats.suvisdev.cloud — Arda(ATS) 팀 문서 사이트

## 정체
- **Arda(채용 지원자 관리 시스템) 팀 프로젝트의 공식 문서 사이트**다.
  팀 Seuk(5명) 공용 — 회사 발표·평가자 열람용.
- 개인 포트폴리오(jk.suvisdev.cloud, `~/suvisdev.cloud/suvisjk`)와 별개다.
  **개인 프로젝트(Mova·Gildle 등) 콘텐츠는 여기 넣지 않는다.** 반대로
  Arda/ATS 콘텐츠는 개인 지킬이 아니라 항상 이 저장소에 넣는다
  (2026-09-03 확정 방침 — suvisjk/CLAUDE.md 상단 블록과 쌍).
- 코드 저장소는 `Team-Seuk/Arda`(private), 데모는 arda.seuk.cloud.

## 기술
- Jekyll + **just-the-docs** 테마(dark), Ruby는 rbenv.
- 로컬 확인: `bundle exec jekyll build` (서버는 `serve --host 0.0.0.0`).
- `git push` → GitHub Actions가 Pages로 자동 배포(약 1분). 커밋이 곧 배포다.

## 구조
- 페이지: `index`(표지) · `toc` · `about`(프로젝트 소개 — 아키텍처·ERD·화면)
  · `overview`(사업 개요) · `requirements` · `guidelines` · `schedule`
  · `kanban` · `devlog` · `devlog-details` · `feedback` · `appendix`
- `_posts/` — 개발기 포스트. **devlog 타임라인이 칸반 done 카드와 포스트를
  날짜 기준으로 자동 병합**하므로, 포스트만 푸시해도 타임라인에 반영된다.
- `_data/kanban/<owner>.yml` — **팀원별 소유 파일. 자기 파일만 수정한다**
  (충돌 방지 규칙, 파일 상단 경고 참조). 카드 필드:
  `title/feature/status(doing|done)/done(날짜)/note/detail(펼침 상세)`.
- `_data/feedback/<owner>.yml` — 팀원별 피드백, 같은 소유 규칙.

## 팀 (도메인 오너제, ADR-0007 — 2026-09-03 역할 개편)
| owner | 이름 | 도메인 |
|---|---|---|
| suvisdev | 진수택 (팀장) | **인프라·총괄(AWS)** ← 이 머신의 사용자 |
| woojeongalex | 이우정 | 백엔드 |
| cloverky | 박소연 | 에이전트 |
| minahdev | 김민아 | 앱·프론트 |

이재우(bestcow)는 팀에서 빠짐 — 칸반의 과거 done 카드는 타임라인 이력
보존을 위해 그대로 둔다(현행 역할 표에는 미등재).

이 머신에서 작업할 때는 `_data/kanban/suvisdev.yml`·`_data/feedback/suvisdev.yml`만
수정한다. 다른 팀원 파일은 읽기 전용.

## 작성 규칙
- **발표용 톤** — 외부(평가자·회사)에 그대로 보여주는 문서. 내부 작업 용어
  ("피드백 반영" 등) 대신 발표 표현("기능 확정", "검토 결과")을 쓴다.
- **전문적이되 누구나 읽게** (2026-09-03 사용자 지시) — 전문 용어를 쓰면
  짧은 풀이를 함께 단다(예: "presigned URL(일회용 업로드 허가 주소)",
  "revert(문제 커밋 되돌리기)"). 비개발자 평가자가 읽어도 흐름이 끊기지
  않아야 한다. 팀 내부 사정(이탈·권한 인수 등)은 싣지 않는다.
- 요약표 먼저, 상세는 뒤에.
- 이미지는 `assets/img/`에 두고 절대경로(`/assets/img/...`)로 참조한다.
- `index.markdown`의 **문서 작성일은 2026-08-20 고정**(자동 갱신 제거,
  커밋 f09ba62) — 건드리지 않는다.
- 마일스톤 게이트 2개가 일정의 축: **09/04 초기 버전**(외부에 보여줄 수
  있는 최소 동작) · **09/30 1차 완성**. 팀장 머지·검수는 매주 금요일.
