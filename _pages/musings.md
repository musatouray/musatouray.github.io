---
layout: single
title: "Musings"
permalink: /musings/
author_profile: true
classes: wide
---

<div class="musings-grid" id="musings-grid">
  {% for post in site.posts %}
  {% assign words = post.content | number_of_words %}
  {% assign mins = words | divided_by: 200 %}
  {% if mins < 1 %}{% assign mins = 1 %}{% endif %}
  <div class="musing-card">
    <a href="{{ post.url | relative_url }}" class="musing-card__image-link">
      {% if post.header.teaser %}
      <img src="{{ post.header.teaser }}" alt="{{ post.title }}" loading="lazy">
      {% else %}
      <div class="musing-card__no-image"></div>
      {% endif %}
    </a>
    <div class="musing-card__body">
      <h3 class="musing-card__title">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h3>
      <p class="musing-card__meta">
        <span>{{ post.date | date: "%b %d, %Y" }}</span>
        <span>&bull; {{ mins }} min read</span>
      </p>
      <p class="musing-card__excerpt">{{ post.excerpt | strip_html | truncate: 130 }}</p>
    </div>
  </div>
  {% endfor %}
</div>
