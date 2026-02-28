---
layout: single
title: "Musa Touray"
classes: wide light-hero
author_profile: false
header:
  overlay_color: "#f0f4f8"
  overlay_filter: "1"
  actions:
    - label: "Connect on LinkedIn"
      url: "https://www.linkedin.com/in/musa-touray/"
excerpt: "Data Engineer &middot; Power BI Developer &middot; Microsoft Fabric"
---

## Featured Projects

<div class="projects-grid">
  {% assign featured = site.portfolio | sort: "date" | reverse %}
  {% for project in featured %}
  {% assign tag_slugs = "" %}
  {% for tag in project.tags %}
    {% assign slug = tag | downcase | replace: " ", "-" | replace: "_", "-" %}
    {% assign tag_slugs = tag_slugs | append: slug | append: " " %}
  {% endfor %}
  <div class="project-card" data-tags="{{ tag_slugs | strip }}">
    <a href="{{ project.url | relative_url }}" class="project-card__image-link">
      {% if project.header.teaser %}
      <img src="{{ project.header.teaser }}" alt="{{ project.title }}" loading="lazy">
      {% else %}
      <div class="project-card__no-image"></div>
      {% endif %}
    </a>
    <div class="project-card__body">
      {% if project.tags.size > 0 %}
      <div class="project-card__tags">
        {% for tag in project.tags limit:2 %}
        <span class="project-tag">{{ tag }}</span>
        {% endfor %}
      </div>
      {% endif %}
      <h3 class="project-card__title">
        <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
      </h3>
      <p class="project-card__excerpt">{{ project.excerpt | strip_html | truncate: 90 }}</p>
      <a href="{{ project.url | relative_url }}" class="project-card__link">View Project &rarr;</a>
    </div>
  </div>
  {% endfor %}
</div>

<div style="text-align:center; margin: -1rem 0 2.5rem;">
  <a href="/projects/" class="btn btn--primary">View All Projects</a>
</div>
