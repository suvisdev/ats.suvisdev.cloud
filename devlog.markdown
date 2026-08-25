---
layout: default
title: 개발 로그
nav_order: 8
permalink: /devlog/
---

# 개발 로그

{% for post in site.posts reversed %}
<div style="margin-bottom: 1.2rem; padding: 1rem 1.2rem; border-left: 3px solid #444; background: rgba(255,255,255,0.03); border-radius: 0 8px 8px 0;">
  <p style="font-size: 0.8rem; color: #999; margin: 0;">{{ post.date | date: "%Y년 %m월 %d일" }}</p>
  <h3 style="margin: 0.2rem 0 0.4rem;"><a href="{{ post.url | relative_url }}" style="color: #e2e2e2; text-decoration: none;">{{ post.title }}</a></h3>
  {% if post.excerpt %}<p style="font-size: 0.85rem; color: #bbb; margin-top: 0.5rem; line-height: 1.5;">{{ post.excerpt | strip_html | truncate: 120 }}</p>{% endif %}
</div>
{% endfor %}

{% if site.posts.size == 0 %}
아직 개발 로그가 없습니다. 프로젝트 진행에 따라 업데이트됩니다.
{% endif %}
