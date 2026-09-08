---
layout: page
title: 프로젝트
description: "기록이 나오는 곳. 각 프로젝트에서 어떤 글이 나왔는지 모아 둡니다."
permalink: /projects/
---

<!-- 글 수는 각 글 front matter의 `project:` 값으로 집계된다. 목록·설명은 _data/projects.yml. -->

<div class="project-page-list">
{% for project in site.data.projects %}
  {%- assign entries = site.posts | where: "project", project.key -%}
  <section class="project-entry" id="{{ project.key }}">
    <div class="project-entry-head">
      <h2>{{ project.name }}</h2>
      <span class="project-count">기록 {{ entries | size }}편</span>
    </div>
    <p class="project-note">{{ project.note }}</p>
    {% if entries.size > 0 %}
    <ul class="project-posts">
      {%- for post in entries %}
      <li>
        <time datetime="{{ post.date | date: '%Y-%m-%d' }}">{{ post.date | date: '%Y.%m.%d' }}</time>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </li>
      {%- endfor %}
    </ul>
    {% else %}
    <p class="project-empty">아직 이 프로젝트로 쓴 기록이 없습니다.</p>
    {% endif %}
  </section>
{% endfor %}
</div>
