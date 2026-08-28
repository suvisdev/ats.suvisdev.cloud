---
layout: default
title: 개발 로그
nav_order: 8
permalink: /devlog/
---

# 개발 로그

## 완료 작업 타임라인

[팀 칸반](/kanban/)에서 `done` 처리된 카드가 완료일 기준으로 자동 집계됩니다 — 따로 쓸 필요 없이 칸반만 갱신하면 됩니다.

<style>
.dl-day { margin: 1.2rem 0 0.4rem; font-size: 0.85rem; font-weight: 700; opacity: 0.75; }
.dl-row { display: flex; flex-wrap: wrap; align-items: baseline; gap: 7px; padding: 5px 0 5px 10px; border-left: 3px solid rgba(255,255,255,0.15); font-size: 0.85rem; }
.dl-owner { font-size: 0.66rem; font-weight: 700; padding: 1px 8px; border-radius: 999px; color: #111; white-space: nowrap; }
.dl-feature { font-size: 0.66rem; padding: 1px 7px; border-radius: 4px; border: 1px solid rgba(255,255,255,0.2); opacity: 0.8; white-space: nowrap; }
.dl-note { font-size: 0.74rem; opacity: 0.55; width: 100%; padding-left: 2px; }
</style>

{% assign entries = "" | split: "" %}
{% for member in site.data.kanban %}{% assign m = member[1] %}{% for card in m.cards %}{% if card.status == "done" and card.done %}
{% capture row %}{{ card.done }}¦{{ m.color }}¦{{ m.name | default: m.owner }}¦{{ card.feature }}¦{{ card.title }}¦{{ card.note }}{% endcapture %}
{% assign entries = entries | push: row %}
{% endif %}{% endfor %}{% endfor %}
{% assign entries = entries | sort | reverse %}
{% assign prev_day = "" %}
{% for row in entries %}{% assign p = row | split: "¦" %}
{% if p[0] != prev_day %}<div class="dl-day">{{ p[0] }}</div>{% assign prev_day = p[0] %}{% endif %}
<div class="dl-row">
  <span class="dl-owner" style="background: {{ p[1] }};">{{ p[2] }}</span>
  {% if p[3] and p[3] != "" %}<span class="dl-feature">{{ p[3] }}</span>{% endif %}
  <span>{{ p[4] }}</span>
  {% if p[5] and p[5] != "" %}<span class="dl-note">{{ p[5] }}</span>{% endif %}
</div>
{% endfor %}
{% if entries.size == 0 %}
<p style="opacity:0.5;">아직 완료 기록이 없습니다.</p>
{% endif %}

---

## 포스트

{% assign prev_pday = "" %}
{% for post in site.posts %}
{% assign pday = post.date | date: "%Y-%m-%d" %}
{% if pday != prev_pday %}<div class="dl-day">{{ pday }}</div>{% assign prev_pday = pday %}{% endif %}
<div class="dl-row">
  <span class="dl-feature">포스트</span>
  <a href="{{ post.url | relative_url }}" style="color: #e2e2e2;">{{ post.title }}</a>
</div>
{% endfor %}

{% if site.posts.size == 0 %}
아직 개발 로그가 없습니다. 프로젝트 진행에 따라 업데이트됩니다.
{% endif %}
