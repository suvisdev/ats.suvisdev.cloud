---
layout: default
title: 팀 칸반
nav_order: 9
permalink: /kanban/
---

# 팀 칸반 보드

개발 기간(W1~W5) 동안의 팀원별 백로그·진행 현황. **카드는 `_data/kanban/<자기 GitHub 아이디>.yml`에서만 편집한다** — 한 사람이 파일 하나를 소유하므로 5명이 동시에 작업해도 git 충돌이 나지 않는다.

<style>
.kb-board { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin: 1rem 0 2rem; }
@media (max-width: 768px) { .kb-board { grid-template-columns: 1fr; } }
.kb-col { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.1); border-radius: 8px; padding: 10px; }
.kb-col-title { font-weight: 700; font-size: 0.8rem; letter-spacing: 0.05em; text-transform: uppercase; opacity: 0.8; margin: 2px 0 10px 4px; }
.kb-card { border: 1px solid rgba(255,255,255,0.12); border-left-width: 4px; border-radius: 6px; padding: 8px 10px; margin-bottom: 8px; background: rgba(0,0,0,0.25); }
.kb-card-title { font-size: 0.82rem; line-height: 1.45; margin-bottom: 5px; }
.kb-meta { display: flex; flex-wrap: wrap; gap: 5px; align-items: center; }
.kb-owner { font-size: 0.66rem; font-weight: 700; padding: 1px 8px; border-radius: 999px; color: #111; }
.kb-tag { font-size: 0.66rem; padding: 1px 7px; border-radius: 4px; border: 1px solid rgba(255,255,255,0.2); opacity: 0.85; }
.kb-due { font-size: 0.66rem; opacity: 0.65; }
.kb-note { font-size: 0.7rem; opacity: 0.6; margin-top: 4px; }
.kb-legend { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 0.5rem; }
.kb-count { opacity: 0.6; font-weight: 400; }
</style>

<div class="kb-legend">
{% for member in site.data.kanban %}{% assign m = member[1] %}
  <span class="kb-owner" style="background: {{ m.color }};">{{ m.owner }} · {{ m.domain }}</span>
{% endfor %}
</div>

<div class="kb-board">
{% assign columns = "todo:할 일,doing:진행 중,done:완료" | split: "," %}
{% for col in columns %}
  {% assign parts = col | split: ":" %}
  {% assign status = parts[0] %}
  <div class="kb-col">
    {% assign total = 0 %}
    {% for member in site.data.kanban %}{% assign m = member[1] %}{% for card in m.cards %}{% if card.status == status %}{% assign total = total | plus: 1 %}{% endif %}{% endfor %}{% endfor %}
    <div class="kb-col-title">{{ parts[1] }} <span class="kb-count">({{ total }})</span></div>
    {% for member in site.data.kanban %}
      {% assign m = member[1] %}
      {% for card in m.cards %}
        {% if card.status == status %}
        <div class="kb-card" style="border-left-color: {{ m.color }};">
          <div class="kb-card-title">{{ card.title }}</div>
          <div class="kb-meta">
            <span class="kb-owner" style="background: {{ m.color }};">{{ m.owner }}</span>
            {% if card.feature %}<span class="kb-tag">{{ card.feature }}</span>{% endif %}
            {% if card.due %}<span class="kb-due">~{{ card.due }}</span>{% endif %}
          </div>
          {% if card.note %}<div class="kb-note">{{ card.note }}</div>{% endif %}
        </div>
        {% endif %}
      {% endfor %}
    {% endfor %}
  </div>
{% endfor %}
</div>

## 사용 규칙 (충돌 방지)

1. **자기 파일만 수정한다.** 카드 데이터는 `_data/kanban/<GitHub 아이디>.yml` — 본인 소유 파일 외에는 손대지 않는다. 남의 카드에 할 말이 있으면 팀 채널로.
2. **카드 형식**은 파일 안 기존 항목을 복사해서 쓴다. `status`는 `todo | doing | done` 세 값만.
   ```yaml
   - title: "카드 제목"
     feature: "D3"        # 기능 번호 또는 주차 (선택)
     status: todo         # todo | doing | done
     due: 2026-09-04      # 마감 (선택)
     note: ""             # 한 줄 메모 (선택)
   ```
3. **push 전 `git pull --rebase`.** 서로 다른 파일이라 rebase가 항상 깨끗하게 통과한다 — 같은 파일을 여럿이 고칠 때만 충돌이 나는데, 이 구조에서는 그 경우가 없다.
4. **포스트(`_posts/`)도 같은 원칙**: 파일명을 `YYYY-MM-DD-<아이디>-<주제>.markdown`으로 만들어 한 파일을 한 사람만 만지게 한다.
5. `done` 카드는 주간 다이제스트(금요일) 이후 각자 정리(삭제 또는 유지)한다.
