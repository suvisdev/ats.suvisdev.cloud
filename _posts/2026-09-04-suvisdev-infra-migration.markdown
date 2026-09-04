---
layout: default
title: "인프라 완전 이전 — 개인 AWS·자동 배포·팀 셀프서비스 개통"
date: 2026-09-04
categories: [devlog, infra]
owner: 진수택
owner_color: "#7aa2f7"
summary: "이탈자 명의 AWS를 떠나 개인 계정(서울)으로 전체 이전 — EC2 arda-api(t3.small)·S3·SQS·SES·IAM 3단 분리, Caddy HTTPS(api.seuk.suvisdev.cloud). main 머지→2분 폴링 자동 배포 개통, 브랜치 규칙 확정(직접 푸시 금지·PR 셀프 머지). 더미 15명 리허설로 presign 글로벌 호스트 서명 버그 실전 검출·수정(PR #4). 저장소 Seuk-Team org 통합(Arda·jekyll)"
---

> 팀장 이탈로 남의 명의에 얹혀 있던 운영 전부를 하루 만에 내 AWS로 옮기고,
> "배포해 주세요"가 필요 없는 팀 셀프서비스 체계(각자 커밋→머지→몇 분 내 반영)까지 개통했다.

## 무엇이 옮겨졌나

- **컴퓨트**: 신규 EC2 `arda-api`(t3.small, Ubuntu 24.04, Elastic IP) — 옛 t3.micro 실측의 2배 사양, 스왑 2G 관례 유지
- **스토리지·메일**: S3 `arda-resumes-seuk`(비공개+CORS) · SQS `arda-mail` · SES 도메인 DKIM 인증(프로덕션 해제 신청 중 — 그때까지 dry-run)
- **권한 3단 분리**: 콘솔 관리(suvisdev, MFA) / 팀 열람(`arda-viewers` ViewOnlyAccess — 이력서 다운로드 불가) / 서버 키(`arda-server` — Arda 리소스 한정, 새어도 폭발 반경 제한)
- **주소**: 서비스 seuk.suvisdev.cloud · API api.seuk.suvisdev.cloud (Caddy가 Let's Encrypt 직접 발급)
- **저장소**: `Seuk-Team` org로 통합 — Arda(main 동기화+브랜치 13개), 이 문서 사이트(`Seuk-Team/jekyll`로 transfer·개명, Pages·도메인 무중단 생존)

## 배포가 자동이 됐다

서버가 2분마다 main을 보고 새 커밋이면 pull→build→up→헬스체크(systemd 타이머).
빌드와 up을 분리해 빌드가 깨져도 돌던 컨테이너는 살아남는다 — 옛 07-deploy의
디스크 고갈 교훈을 스크립트에 박은 것. 브랜치 규칙도 확정: **main 직접 푸시
금지, 브랜치→PR, 승인 없이 셀프 머지**(CI 초록일 때만). 오늘 하루 PR 5건이
이 파이프라인으로 흘러갔다.

## 더미 리허설이 실전 버그를 잡았다

더미 지원자 15명(공고 2건, 이력서+자소서 PDF 30건)을 실제 플로우(presign→S3
직행 PUT→제출)로 넣다가 전멸 — 원인은 더미가 아니라 **presign이 S3 글로벌
호스트로 서명하는 백엔드 버그**였다. 갓 만든 버킷은 글로벌 호스트가 몇 시간
동안 307을 돌려주고, 따라가도 서명 리전 불일치로 403. 실제 지원자가 브라우저로
올렸어도 똑같이 실패했을 것. 리전 엔드포인트 명시로 수정(PR #4) 후 15/15 관통,
S3 30객체·DB 15건 실측. 리허설은 데모 준비가 아니라 배포 검증이었다.

## 그 외

- pytest가 로컬에서 24분 침묵한 사건 — 원인은 Docker Desktop 다운으로 인한 DB
  connect 무한 대기. `pytest-timeout` 60초를 dev 그룹에 잠가(PR #5) hang을
  "어느 테스트가 어디서 기다렸는지"가 보이는 실패로 바꿨다
- 서비스 admin 4명 체제(전 팀원 계정 발급), 온보딩은 `docs/00_overview/10-team-setup.md` 한 장으로
