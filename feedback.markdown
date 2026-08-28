---
layout: default
title: 피드백 트래커
nav_order: 10
permalink: /feedback/
---

# 피드백 트래커

<style>
.fb-table { width: 100%; border-collapse: collapse; font-size: 0.8rem; margin: 1rem 0 2rem; }
.fb-table th, .fb-table td { border: 1px solid rgba(255,255,255,0.12); padding: 7px 9px; vertical-align: top; text-align: left; }
.fb-table th { font-size: 0.72rem; letter-spacing: 0.04em; opacity: 0.8; white-space: nowrap; }
.fb-owner { font-size: 0.66rem; font-weight: 700; padding: 1px 8px; border-radius: 999px; color: #111; white-space: nowrap; }
.fb-status { font-size: 0.66rem; font-weight: 700; padding: 1px 8px; border-radius: 4px; white-space: nowrap; }
.fb-todo { background: rgba(247,118,142,0.2); color: #f7768e; }
.fb-doing { background: rgba(224,175,104,0.2); color: #e0af68; }
.fb-done { background: rgba(158,206,106,0.2); color: #9ece6a; }
.fb-from { font-size: 0.68rem; opacity: 0.6; }
.fb-date { white-space: nowrap; }
details { margin-top: 1.5rem; }
details summary { cursor: pointer; font-size: 0.78rem; opacity: 0.6; }
details summary:hover { opacity: 1; text-decoration: underline; }
</style>

<table class="fb-table">
  <thead>
    <tr>
      <th>접수일</th>
      <th>피드백 내용</th>
      <th>담당</th>
      <th>상태</th>
      <th>수정일</th>
      <th>수정 내용</th>
    </tr>
  </thead>
  <tbody>
{% assign rows = "" | split: "" %}
{% for member in site.data.feedback %}{% assign m = member[1] %}{% for item in m.feedback %}
    <tr>
      <td class="fb-date">{{ item.received }}</td>
      <td>{{ item.content }}{% if item.from %}<div class="fb-from">출처: {{ item.from }}</div>{% endif %}</td>
      <td><span class="fb-owner" style="background: {{ m.color }};">{{ m.name | default: m.owner }}</span></td>
      <td>{% case item.status %}{% when "done" %}<span class="fb-status fb-done">반영 완료</span>{% when "doing" %}<span class="fb-status fb-doing">수정 중</span>{% else %}<span class="fb-status fb-todo">대기</span>{% endcase %}</td>
      <td class="fb-date">{{ item.fixed | default: "—" }}</td>
      <td>{{ item.fix | default: "—" }}</td>
    </tr>
{% endfor %}{% endfor %}
{% assign total = 0 %}{% for member in site.data.feedback %}{% assign m = member[1] %}{% assign total = total | plus: m.feedback.size %}{% endfor %}
{% if total == 0 %}
    <tr><td colspan="6" style="opacity:0.5; text-align:center;">아직 기록된 피드백이 없습니다.</td></tr>
{% endif %}
  </tbody>
</table>

<details markdown="1">
<summary>📖 사용 규칙 보기 (기록 방법)</summary>

기록은 피드백을 처리하는 담당자가 `_data/feedback/<자기 GitHub 아이디>.yml`에 남긴다 — 칸반과 같은 원칙(한 사람 = 파일 하나)이라 git 충돌이 없다.

1. **자기 파일만 수정한다.** 담당이 애매한 피드백은 팀 채널에서 담당을 정한 뒤 그 사람이 기록한다.
2. **새 항목은 파일 맨 위에 추가**한다(최신순 유지). 형식:
   ```yaml
   - received: 2026-09-01      # 피드백 들어온 날짜
     from: "09/04 중간점검"     # 출처 (선택 — 발표·데모·팀원·외부)
     content: "피드백 내용"
     status: todo               # todo(대기) | doing(수정 중) | done(반영 완료)
     fixed:                     # 수정한 날짜 — 반영 완료 시 기입
     fix: ""                    # 수정한 내용 — 반영 완료 시 기입
   ```
3. 접수 시점에는 `received/from/content/status: todo`만 채우고, **반영이 끝나면 같은 항목에 `fixed`·`fix`를 채우고 `status: done`으로 바꾼다** — 접수와 반영이 한 줄에서 추적된다.
4. 반영이 칸반 카드로 이어지면 카드 `note`에 "피드백 반영"이라고 적어 연결한다.

</details>
