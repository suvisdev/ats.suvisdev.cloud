---
layout: default
title: 피드백 트래커
nav_order: 10
permalink: /feedback/
---

# 피드백 트래커

**멘토·강사님 피드백**의 접수와 반영 이력을 추적한다 — 언제 어떤 피드백을 받았고, 언제 무엇을 수정했는지.

<style>
.fb-item { border: 1px solid rgba(255,255,255,0.14); border-radius: 10px; margin: 1rem 0; overflow: hidden; }
.fb-head { display: flex; flex-wrap: wrap; align-items: center; gap: 8px; padding: 10px 14px; background: rgba(255,255,255,0.04); border-bottom: 1px solid rgba(255,255,255,0.1); }
.fb-owner { font-size: 0.7rem; font-weight: 700; padding: 2px 10px; border-radius: 999px; color: #111; white-space: nowrap; }
.fb-status { font-size: 0.7rem; font-weight: 700; padding: 2px 10px; border-radius: 5px; white-space: nowrap; }
.fb-todo { background: rgba(247,118,142,0.2); color: #f7768e; }
.fb-doing { background: rgba(224,175,104,0.2); color: #e0af68; }
.fb-done { background: rgba(158,206,106,0.2); color: #9ece6a; }
.fb-dates { font-size: 0.74rem; opacity: 0.65; margin-left: auto; white-space: nowrap; }
.fb-block { padding: 12px 14px; }
.fb-block + .fb-block { border-top: 1px dashed rgba(255,255,255,0.1); }
.fb-label { font-size: 0.68rem; font-weight: 700; letter-spacing: 0.06em; opacity: 0.55; margin-bottom: 4px; }
.fb-block p { margin: 0; font-size: 0.86rem; line-height: 1.7; }
.fb-fix { border-left: 3px solid #9ece6a; background: rgba(158,206,106,0.05); }
.fb-empty { opacity: 0.5; text-align: center; padding: 2rem 0; font-size: 0.85rem; }
details { margin-top: 1.5rem; }
details summary { cursor: pointer; font-size: 0.78rem; opacity: 0.6; }
details summary:hover { opacity: 1; text-decoration: underline; }
</style>

{% assign total = 0 %}{% for member in site.data.feedback %}{% assign m = member[1] %}{% assign total = total | plus: m.feedback.size %}{% endfor %}
{% if total == 0 %}
<div class="fb-empty">아직 기록된 피드백이 없습니다.</div>
{% endif %}
{% for member in site.data.feedback %}{% assign m = member[1] %}{% for item in m.feedback %}
<div class="fb-item">
  <div class="fb-head">
    <span class="fb-owner" style="background: {{ m.color }};">{{ m.name | default: m.owner }}</span>
    {% case item.status %}{% when "done" %}<span class="fb-status fb-done">반영 완료</span>{% when "doing" %}<span class="fb-status fb-doing">수정 중</span>{% else %}<span class="fb-status fb-todo">대기</span>{% endcase %}
    <span class="fb-dates">접수 {{ item.received }}{% if item.from %} · {{ item.from }}{% endif %}{% if item.fixed %} → 반영 {{ item.fixed }}{% endif %}</span>
  </div>
  <div class="fb-block">
    <div class="fb-label">피드백</div>
    <p>{{ item.content }}</p>
  </div>
  {% if item.fix and item.fix != "" %}
  <div class="fb-block fb-fix">
    <div class="fb-label">반영 내용</div>
    <p>{{ item.fix }}</p>
  </div>
  {% endif %}
</div>
{% endfor %}{% endfor %}

<details markdown="1">
<summary>📖 사용 규칙 보기 (기록 방법)</summary>

멘토링·강의에서 받은 피드백을, **반영을 담당하는 팀원**이 `_data/feedback/<자기 GitHub 아이디>.yml`에 기록한다 — 칸반과 같은 원칙(한 사람 = 파일 하나)이라 git 충돌이 없다.

1. **자기 파일만 수정한다.** 어느 도메인 소관인지 애매한 피드백은 팀 채널에서 담당을 정한 뒤 그 사람이 기록한다.
2. **새 항목은 파일 맨 위에 추가**한다(최신순 유지). 형식:
   ```yaml
   - received: 2026-09-01      # 피드백 들어온 날짜
     from: "09/04 중간점검"     # 출처 (예: 멘토링·강사 피드백·중간점검)
     content: "피드백 내용"
     status: todo               # todo(대기) | doing(수정 중) | done(반영 완료)
     fixed:                     # 수정한 날짜 — 반영 완료 시 기입
     fix: ""                    # 수정한 내용 — 반영 완료 시 기입
   ```
3. 접수 시점에는 `received/from/content/status: todo`만 채우고, **반영이 끝나면 같은 항목에 `fixed`·`fix`를 채우고 `status: done`으로 바꾼다** — 접수와 반영이 한 줄에서 추적된다.
4. 반영이 칸반 카드로 이어지면 카드 `note`에 "피드백 반영"이라고 적어 연결한다.

</details>
