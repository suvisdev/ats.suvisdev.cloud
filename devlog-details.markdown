---
layout: default
title: 완료 작업 상세
nav_exclude: true
permalink: /devlog/details/
---

# 완료 작업 상세

<div class="dd-nav">
  <a href="/devlog/">← 개발 로그로 돌아가기</a>
  <a href="#" id="dd-all" style="display:none;">전체 완료 기록 보기</a>
</div>

<style>
.dd-day { display: flex; align-items: center; gap: 10px; margin: 1.8rem 0 0.7rem; font-size: 0.98rem; font-weight: 800; color: #eceff4; }
.dd-day::before { content: ""; width: 5px; height: 1.1em; border-radius: 2px; background: #7aa2f7; }
.dd-day::after { content: ""; flex: 1; height: 1px; background: rgba(255,255,255,0.14); }
.dd-item { border: 1px solid rgba(255,255,255,0.12); border-left: 4px solid var(--mc); border-radius: 8px; padding: 12px 16px; margin: 0.8rem 0; scroll-margin-top: 80px; }
.dd-item:target { border-color: #7aa2f7; background: rgba(122,162,247,0.06); }
.dd-head { display: flex; flex-wrap: wrap; align-items: center; gap: 8px; margin-bottom: 6px; }
.dd-owner { font-size: 0.7rem; font-weight: 700; padding: 2px 10px; border-radius: 999px; color: #111; }
.dd-feature { font-size: 0.68rem; padding: 1px 8px; border-radius: 4px; border: 1px solid rgba(255,255,255,0.2); opacity: 0.8; }
.dd-title { font-size: 0.95rem; font-weight: 700; margin-bottom: 4px; }
.dd-note { font-size: 0.76rem; opacity: 0.6; margin-bottom: 6px; }
.dd-detail { font-size: 0.84rem; line-height: 1.75; opacity: 0.92; white-space: pre-line; border-top: 1px dashed rgba(255,255,255,0.12); padding-top: 8px; margin-top: 4px; }
.dd-empty { font-size: 0.8rem; opacity: 0.5; }
.dd-nav { display: flex; gap: 16px; margin-bottom: 0.5rem; font-size: 0.85rem; }
.dd-nav a { color: #7aa2f7; text-decoration: none; }
.dd-nav a:hover { text-decoration: underline; }
</style>

{% assign entries = "" | split: "" %}
{% for member in site.data.kanban %}{% assign m = member[1] %}{% for card in m.cards %}{% if card.status == "done" and card.done %}
{% capture row %}{{ card.done }}¦{{ m.color }}¦{{ m.name | default: m.owner }}¦{{ card.feature }}¦{{ card.title }}¦{{ card.note }}¦{{ card.detail }}{% endcapture %}
{% assign entries = entries | push: row %}
{% endif %}{% endfor %}{% endfor %}
{% assign entries = entries | sort | reverse %}
{% assign prev_day = "" %}
{% for row in entries %}{% assign p = row | split: "¦" %}
{% if p[0] != prev_day %}<div class="dd-day">{{ p[0] }}</div>{% assign prev_day = p[0] %}{% endif %}
<div class="dd-item" id="d{{ forloop.index }}" style="--mc: {{ p[1] }};">
  <div class="dd-head">
    <span class="dd-owner" style="background: {{ p[1] }};">{{ p[2] }}</span>
    {% if p[3] and p[3] != "" %}<span class="dd-feature">{{ p[3] }}</span>{% endif %}
  </div>
  <div class="dd-title">{{ p[4] }}</div>
  {% if p[5] and p[5] != "" %}<div class="dd-note">{{ p[5] }}</div>{% endif %}
  {% if p[6] and p[6] != "" %}<div class="dd-detail">{{ p[6] | strip }}</div>{% else %}<div class="dd-empty">상세 요약이 아직 없습니다 — 칸반 카드의 detail 필드에 채우면 여기 표시됩니다.</div>{% endif %}
</div>
{% endfor %}

<script>
(function () {
  function applyFilter() {
    var hash = location.hash.replace("#", "");
    var items = document.querySelectorAll(".dd-item");
    var anyTarget = hash && document.getElementById(hash);
    items.forEach(function (el) {
      el.style.display = (!anyTarget || el.id === hash) ? "" : "none";
    });
    // 날짜 헤더: 다음 헤더 전까지 보이는 카드가 하나라도 있어야 표시
    document.querySelectorAll(".dd-day").forEach(function (day) {
      var visible = false;
      var el = day.nextElementSibling;
      while (el && !el.classList.contains("dd-day")) {
        if (el.classList.contains("dd-item") && el.style.display !== "none") visible = true;
        el = el.nextElementSibling;
      }
      day.style.display = visible ? "" : "none";
    });
    var allBtn = document.getElementById("dd-all");
    if (allBtn) allBtn.style.display = anyTarget ? "" : "none";
  }
  var allBtn = document.getElementById("dd-all");
  if (allBtn) allBtn.addEventListener("click", function (e) {
    e.preventDefault();
    history.replaceState(null, "", location.pathname);
    applyFilter();
  });
  window.addEventListener("hashchange", applyFilter);
  applyFilter();
})();
</script>
